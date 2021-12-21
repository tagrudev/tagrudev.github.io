---
title:  "Sidekiq, systemd and Capistrano"
date: 2019-10-10 08:10
category: dev
tags: [ruby, server]
---

In this blog post I am going to write about my approach on deploying a `Ruby On Rails` application and dealing with `Sidekiq` (job scheduler written in `Ruby`). This approach is probably suitable for a small/medium application like  [Todor Grudev - Outmuscle Me](https://tagrudev.com/2019/outmuscle-me/){:target="_blank"}. I would not recommend following this for a project with a lot of usage, Outmuscle Me at the time of that post has 262 Users, so you can imagine the infrastructure is not really demanding.  Heavy operations like syncing activities with Garmin and Fitbit were actually the reasons I decided to rollout `Sidekiq` in the first place.

The current setup consists of  a single server that takes care of serving the `Rails` content, storing the database and running the `Sidekiq`. Yeah, I am aware that this is not a good idea, but yet again I am running a start up here :). Deploying the app with `Capistrano` (a tool for running scripts) and in order to run `Sidekiq` I am using `systemd`. If you are not familiar with `systemd`, here is how Wiki describes it:

> The systemd software suite provides fundamental building blocks for a Linux operating system. It includes the systemd "System and Service Manager", an init system used to bootstrap user space and manage user processes.

Check [A brief overview and history of systemd — the Linux process manager - By](https://hackernoon.com/a-brief-overview-and-history-of-systemd-the-linux-process-manager-ca508bee4a33){:target="_blank"} for more information, the important part of this sentence is that it manages user processes. In my case it will take care of keeping `Sidekiq` alive. It will also take care of stoping it gracefully. My server is running Ubuntu, things may vary based on the operating system (ex. the folder you put the service script in). I am also using `rbenv` to control the `Ruby` versions so you may need to change some paths in the configurations below. Another prerequisite is having a `deploy` user that `Capistrano` would use. I believe that if you are searching the internet and have reached this article you probably already know all those stuff, so let's jump into the necessesary steps needed to do the magic:

### Step 1 - Create a service

Add a new file in  `/lib/systemd/system` called `sidekiq.service`, content:

```bash
[Unit]
Description=Outmuscle Me Production Sidekiq
After=syslog.target network.target

[Service]
WorkingDirectory=/data/apps/outmuscleme/current

User=deploy
Group=deploy

Environment=RBENV_ROOT=/usr/local/rbenv
EnvironmentFile=/data/apps/outmuscleme/shared/.env

ExecStart=/usr/local/rbenv/bin/rbenv exec bundle exec sidekiq -e production

RestartSec=1
Restart=on-failure

SyslogIdentifier=sidekiq

[Install]
WantedBy=multi-user.target
```

### Step 2 - Enable the service

```bash
/bin/systemctl enable sidekiq
```

Here `systemctl`  is a command, which is the central management tool for controlling the init system.

### Step 3 - Allow the deploy user to restart / kill

As a root user type `visudo` it will open up the `vi` editor loaded with the current `/etc/sudoers` file, at the end of the file you can add the following (or preferably you can create a new file at `/etc/sudoers.d/sidekiq-users` and add it  there)

```bash
%deploy ALL= NOPASSWD: /bin/systemctl restart sidekiq
%deploy ALL= NOPASSWD: /bin/systemctl kill -s TSTP sidekiq
```

### Step 4 - Modify Capistrano's deploy file (`config/deploy.rb`)

```ruby
set :sidekiq_service_name, "sidekiq"

namespace :sidekiq do
  desc "Quiet sidekiq (stop fetching new tasks from Redis)"
  task :quiet do
    on roles(:app) do
      execute :sudo, :systemctl, :kill, "-s", "TSTP", fetch(:sidekiq_service_name)
    end
  end

  desc "Restart sidekiq service"
  task :restart do
    on roles(:app) do
      execute :sudo, :systemctl, :restart, fetch(:sidekiq_service_name)
    end
  end
end

after "deploy:starting", "sidekiq:quiet"
after "deploy:published", "sidekiq:restart"
```

According to `Sidekiq` 's documentation - TSTP tells `Sidekiq` to "quiet" as it will be shutting down at some point in the near future. It will stop fetching new jobs but continue working on current jobs. TSTP + TERM guarantee shutdown within a time period. Best practice is to send TSTP at the start of a deploy and TERM at the end of a deploy. So as you can see we are doing the `quiet` operation at the starting of the deployment and the terminating / restarting at the publishing state. That's it, as a final step verify that everything is running correctly.

I would really appreciate some feedback and I would love to see how you've setup the background processing of you application, leave me a comment ;)

Additional sources:
<br>
[Sidekiq in Production | playbook](https://curationexperts.github.io/playbook/production/sidekiq_in_production.html)
<br>
[Sidekiq's signals](https://github.com/mperham/sidekiq/wiki/Signals)
<br>
[Sidekiq's service example](https://github.com/mperham/sidekiq/blob/master/examples/systemd/sidekiq.service)

