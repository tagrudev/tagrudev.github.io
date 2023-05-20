---
title:  "Mimic the iOS phased release in Android"
date: "2023-05-20 13:52"
description: Work in progress
category: mobile
tags: [android, ruby]
---
## Wait, what’s a phased release?
A phased release, or rollout, is a process where an app update is released to customers in stages instead of all at once. This is useful in several ways:

1. Gauge how customers respond to an app update and respond accordingly
2. Backend processes or infrastructure can be scaled intelligently as adoption ramps up
If something very unexpected happens, the release can be paused for triage leaving only X% of customers affected instead of everyone

While this feature is both available in the iOS and Android world, it's not done automatically for the Androids...

## How the App Store (iOS) phased releases work?

When you enable a phased release in the App Store your app update is gradually released to customers with automatic updates turned on.
<table> <thead> <tr> <th style="text-align: left">Day</th> <th style="text-align: left">Release Percentage</th> </tr> </thead> <tbody> <tr> <td style="text-align: left">Day 1</td> <td style="text-align: left">1%</td> </tr> <tr> <td style="text-align: left">Day 2</td> <td style="text-align: left">2%</td> </tr> <tr> <td style="text-align: left">Day 3</td> <td style="text-align: left">5%</td> </tr> <tr> <td style="text-align: left">Day 4</td> <td style="text-align: left">10%</td> </tr> <tr> <td style="text-align: left">Day 5</td> <td style="text-align: left">20%</td> </tr> <tr> <td style="text-align: left">Day 6</td> <td style="text-align: left">50%</td> </tr> <tr> <td style="text-align: left">Day 7</td> <td style="text-align: left">100%</td> </tr> </tbody> </table>

This is handled by Apple automactically, meaning that you don't have to login and update them manually every day, and that's exactly the case for the Android Store. In order to address this problem we are going to use the following:

- **#1** Jenkins or whatever CI you're using, we need something that can guarantee us that the script we are going to write is going to run every day (cronjob will do the same thing)
- **#2** A simple ruby script that will take care of checking what is the current stage of the release and updating it to the next one accordingly

Here's the ruby script I wrote in order to achieve **#2**, I am going to comment most of the lines so it's obvious what's happening

{% highlight ruby %}
# We need those 2 dependencies, you can install them by adding those
# 2 lines in your Gemfile, and then running bundle install, google
# auth is the dependency we need in order to authenticate and the apis
# one is the client wrapper which will handle the update for us.
#
# gem 'googleauth'
# gem 'google-apis-androidpublisher_v3', '~> 0.3.0'
#

require 'google/apis/androidpublisher_v3'
require 'googleauth'

class AndroidPublisherClient
  STATUS_COMPLETED = 'completed'
  STATUS_IN_PROGRESS = 'inProgress'

  # CHANGE ME - this is your app package
  PACKAGE_NAME = 'com.mypackage.app'
  # CHANGE ME - you can test this by changing the release stage
  RELEASE_STAGE = 'production'
  SCOPE = 'https://www.googleapis.com/auth/androidpublisher'

  Androidpublisher = Google::Apis::AndroidpublisherV3 # just a reassign

  attr_reader :percentage

  def initialize
    @publisher = Androidpublisher::AndroidPublisherService.new

    # There are several ways to do the authentication in here, head to
    # https://github.com/googleapis/google-auth-library-ruby in order
    # to check which one is the most appropriate for you
    @authorizer =
      Google::Auth::ServiceAccountCredentials.make_creds(
        # CHANGE ME based on the authentication strategy
        json_key_io: File.open('my_credential_file.json'),
        scope: SCOPE,
      )
    @publisher.authorization = @authorizer
    @percentage = nil
  end

  def update_rollout
    # Every update in the Android world is based on the "edit" notion,
    # you create a new edit then access the track and update it, once
    # you are ready you update and commit the change
    new_edit = @publisher.insert_edit PACKAGE_NAME
    track = @publisher.get_edit_track PACKAGE_NAME, new_edit.id, RELEASE_STAGE
    release_in_progress = track.releases.find { |it| it.status == STATUS_IN_PROGRESS }

    # If we have release in progress, then check how much we need to
    # bump up the version this is following the 7days approach of Apple
    if release_in_progress
      @percentage =
        case release_in_progress.user_fraction
        when 0 then 0.01
        when 0.01 then 0.02
        when 0.02 then 0.05
        when 0.05 then 0.1
        when 0.1 then 0.2
        when 0.2 then 0.5
        when 0.5 then 1
        else
          raise StandardError.new 'Percentage out of sync'
        end

      # If this is the last step, close all the open release
      # and compelete the current one
      if @percentage == 1
        release_in_progress.update! user_fraction: nil, status: STATUS_COMPLETED

        # Deleted other version codes if completed because only allowed
        # one completed version in a release
        track.releases.delete_if {
          |r| !(r.version_codes || []).map(&:to_s).include?(release_in_progress.version_codes.first)
        }
      else
        release_in_progress.update! user_fraction: @percentage
      end

      # so once we are ready we can update the edit track and commit it.
      @publisher.update_edit_track PACKAGE_NAME, new_edit.id, RELEASE_STAGE, track
      @publisher.commit_edit PACKAGE_NAME, new_edit.id

      puts '+------+------------------+-------------+'
      puts "|  Progress updated to: #{@percentage}%  |"
      puts '+------+------------------+-------------+'
    else
      # If there's no current release in progress,
      # delete the current edit and display a message
      @publisher.delete_edit PACKAGE_NAME, new_edit.id
      puts '+------+------------------+-------------+'
      puts "|    No current release in progress     |"
      puts '+------+------------------+-------------+'
    end
  end
end
{% endhighlight %}

Once you have this script you can run it every day in your CI or a cronjob, using the following

```ruby
  publisher = AndroidPublisherClient.new
  publisher.update_rollout
```

Let me know if that's been helpful, leave a comment if you think I can improve the script.

## Usefull links

[How to use App Store Phased Releases
](https://docs.departures.to/appstore/how-to-use-app-store-phased-releases)

[Google Auth Library for Ruby
](https://github.com/googleapis/google-auth-library-ruby)