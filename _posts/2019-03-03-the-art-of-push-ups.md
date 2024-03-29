---
title:  "The Art of Push Ups"
date: 2019-03-03 20:00
category: health
tags: [fitness, health]
---

*A push-up (or press-up) is a common calisthenics exercise beginning from the prone position, or the front leaning rest position. They are also a common form of punishment used in the military, school sport, or in some martial arts disciplines.*

Picked those 2 lines out of Wikipedia, I was surprised and amused that it is a form of punishment, but that's not the theme of the current post. We are going to talk about how to try and achieve consistency... I will introduce you to a challenge that I made last year, will also show you a tool developed in order to track progress. At the end of the post if you feel like going for it, I would be happy to send you an invite :fingers_crossed:

Most of you, that are following me on Facebook know that I've been doing these challenges for several years now, what you don't know is that I am also focused on some that are not that long in terms of period of time, like: I went out and did my first Ultra Marathon last year (this was something that I've built for couple of years) or stop coffee for a month or two, another one was *Do 50 push ups every day for 4 months*.

I will iterate on the last one, 50 is not a big number, what mattered though is that you need to be consistent, building a habbit is hard and that is exactly the objective of the challenge - meaning I'd forgotten on several occasions and did my sets in the office or got out of bed at 23:30 for quick 50.

It all started with a bet. What we did (me and a very close friend of mine - Stefan) in order to track progress was, when one of us finishes the repetitions for the day, he would send a Viber text to the other with: 20x15x15 - number of sets and the repetitions. Funny thing, if you check our communication for the past months, you will see a gibberish of those numbers.

That Viber text was my kick for the day, when I received one I knew Stefan had done his part of the day and it was time for me to get moving. Some of the days it was the other way around, having someone involved is always easier than trying to keep an idea going by yourself. We did that till the end of 2018, turned out that just 4 months are not enough to continue on my own. Several months later, as I didn't have the bet going, I was not motivated and it took me incredible mind effort to do a set or two.

So I asked Stefan to get back at the challenge (turned out that it was not going good for him as well). This time I wanted to involve more people, if this is how it worked for me, probably it would be the same for my friends, the only problem out there was how to keep track when more people are involved - Viber was not the option as receiving texts from a person is one thing, but when several are involved it would be a constant phone buzz or it would end up as a muted group conversations (I know we all hate those).

As a software developer I am used of solving that kind of problems, so I was already building the solution in my head:

* it shouldn't be complicated
* it should take less than a minute to fill in
* it should provide the motivational factor of knowing when someone has finished or did a Set
* it should provide basic stats (again part of the motivational factor)

So I sat down and after several hours of coding on a Sunday afternoon, set up a server in Digital Ocean and deployed a Ruby on Rails application that addressed all of the above, here are some screenshots of how it looks (names are covered GDPR you know):

The first screen shows what I call *the Leaderboard*, it has an activity feed as well.

![](/assets/images/1.png)

 Screen #2 shows you a callendar, this one is mine but you can check other people's as well - now I see I am being consistent on doing 2 sets a day, OCD happiness.

  ![](/assets/images/2.png)

I didn't want this one to be as strict as the first challenge and as I mentioned the main goal is to be consistent, some of the folks has missed a day or two, but that's not the issue. The motivation level is kept high as you drop out from *the Leaderboard* if you haven't done anything for 3 consiquent days. The current challenge will continue for 6 months, I would consider it a success if I am not the only one that's still going after that period of time.

So if you feel like being a part of it, send me a PM and I will invite you :)
