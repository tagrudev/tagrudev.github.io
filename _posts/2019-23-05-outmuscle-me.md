---
title:  "Outmuscle Me"
date: 2019-05-23 08:00
category: dev
tags: [rails, ruby, projects]
---

A couple of months ago I wrote [The Art of Push Ups](https://tagrudev.com/2019/the-art-of-push-ups/){:target="_blank"}, in that blog post I presented a tool that I've developed in order to keep track of my training activities. Surprisingly it grew on its own, there are more than 30 active users, not that many, but enough to give me a push to develop it a little bit further.

In the current blog post I will present you the upgraded version of the Push Ups Challenge, I've named the tool [Outmuscle Me](https://outmuscle.me){:target="_blank"} and it has a more general idea, I will describe the process that I went through while building it, so hang on :)

## How it got made

I am a big fan of building habits, an even bigger fan of building tools that help me in those endeavours. After I released *The Push Ups Challenge*, I got feedback from several users - *"Hey, this is nice, but I do sit ups as well, can I keep track of those?", "I would like to invite some of my friends is that possible?", "Is there a way to add a section that will help us communicate in there, like reddit you know?", "I am not a big fan of facebook, can I use something else to register?"...*

So having those in mind, I made a plan on upgrading/rebuilding the platform:

* Need to add more exercises
* Definitely need to add more ways for signing up
* It would be nice to allow the users to create their own challenges (probably drop a start and an end date to that as well)
* Socializing is key, so drop a comment section in there

<br>
After more than 10 years in the area of software development I have learnt a thing or two on how to kick off a project, my favourite tool of choice for that kind of things is Ruby on Rails (but no worries, this is not a tech post), I've created a board in Trello (a really nice tool for TODO lists, check it) to keep track of the things I need to build and serve as my project management .

I kept the old tool intact as I didn't want to discrupt my user base and I started from scratch. This time I was more focused on quality and building solid grounds so I've created the database structure and decided to start with 6 main exercises - push ups, sit ups, pull ups, squats, plank and running - those are the main things that I do during my training sessions, it does not require for me to be in a gym or have an additional gear.

Added a sign up via an email and kept the facebook login, I will probably add Google/Twitter as well at some point, here's a sneak peek of it

![](/assets/images/outmuscleme/1.png)

I met my minimum value of product, but I was missing something, I decided to involve a friend of mine that is a really good artist, check his [Behance](https://www.behance.net/atanasfilipov){:target="_blank"} profile, thanks a lot Atanas Filipov for the awesome work you did!! What Nasko gave me was the factor that I was missing - personalization. Here are some of the early prototypes of the things he did and I later added to the system:

![](/assets/images/outmuscleme/2.png)

In the right corner you can see the Logo, it pretty much sums the idea of the tool - building muscles and keeping track of it. On the left side you can see the representations of the exercises (each of them has a nice transition effect that mimics a real movement), those graphics and more are used all across the platform, it really brought a personal touch to the whole thing, here's an example:

![](/assets/images/outmuscleme/3.png)

Once you create a challenge, you are presented with a dashboard/leaderboard that is the main engine of the app, it keeps track and has a point based system that is used in order to evaluate the rank of each user. Another sneak peek:

![](/assets/images/outmuscleme/4.png)

For each challenge there are some major points/functionalities

* Activity feed, presenting the activities added by each user
* Calendar that shows a better view of the User activity
* Comments section which serves as a place to ask questions, post a motivation video or just chat
* Share section, which gives a basic URL that you can share in order to invite more people.

[I've created a small video that shows that functionality](https://youtu.be/vnvsdzhKfYM){:target="_blank"}, I will probably add some more in the near future.

I have to mention that the whole platform is mobile friendly, I did a research on the previous tool and turned out that 50% of the users are using only mobile devices, so this one was a major point in my building process.

## What's next?

There are definitely things I want to add to the project, several ideas are stored in my TODO Trello board, some of them are:

* A better Leaderboard algorithm - currently it's based on points, but I really want to up that a notch by adding an algorithm that keeps the score based on the data that the challenge has
* Notifications - it would be really nice to have some notifications/reminders of some kind
* Add more exercises - definitely add more exercises, I already have several requests for Dips
* Levels or Badges - gamification always helps for the motivation

<br>
## Wrap up

If you've read everything, kudos, you're probably going to join and you are quite pumped up about it - otherwise I hope I got to motivate you by sharing my building process. Keep in mind that this is a side project, I don't have the intention to continuously develop it, but I do want to share it in Product Hunt, so I will probably spam you to give me a thumbs up in there.

I've already created social accounts - [Twitter](https://twitter.com/outmuscleme){:target="_blank"}, [Facebook](https://facebook.com/outmuscleme){:target="_blank"}, [Youtube](https://www.youtube.com/channel/UCBOqsDS3WZyyAL7qjpKsDzw){:target="_blank"} so feel free to follow/like/subscribe. Feedback is really appreciated, I've created an email that you can use to do so: info@outmuscle.me, looking forward to it.

Have an awesome day :)

Thank you,
<br>
Todor Grudev
