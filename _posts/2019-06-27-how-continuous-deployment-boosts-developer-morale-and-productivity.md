---
layout: post
post_author: Chris Nelson
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: How Continuous Deployment Boosts Developer Morale and Productivity
publish_date: 2015-04-01 13:44:00 +0000
feature_post_image: "/uploads/continuous-v1.gif"
post_images: []
slug: how-continuous-deployment-boosts-developer-morale-and-productivity

---
We recently partnered with one our our longest-running clients, [Sharethrough](https://teamgaslight.com/work/sharethrough), to transition their projects to continuous deployment. This practice means that when you push code to the master branch in your repository the system is automatically deployed to production on a successful build. For the non-developers in the audience, this means software users start enjoying the new feature you built right away.  

From a technical standpoint, this is no longer difficult to accomplish. Providers like CodeShip and Heroku make it almost trivial to do. So rather than focusing on technical implementation, I thought it would be more interesting to talk about how this move to continuous delivery has boosted the spirits and productivity of the development team. 

## We want to build things that matter
Almost every developer I come in contact with really wants to make a difference. We want to build things real people use to do something valuable. This is is the whole point of what we do. But all too often we wait weeks, months, or even years to see people use the systems we pour so much of ourselves into building. In the worst cases, the systems we work hard to build never even make it to production at all. It’s hard to state the level of demoralization this causes. I once heard someone say, “It’s not working too hard that burns out developers, it’s not shipping.” 

If you’re a developer, you know this all too well. But before making this transition to continuous delivery, I don’t think I understood as clearly how much shipping constantly boosts a dev team. At Sharethrough, we were by no means anywhere close to the worst of the situations I’m describing. We had a process pretty similar to git flow; we created a branch for each story we worked on. When it was “done,” we’d do a pull request, and the team lead would review then merge into master. 

Early in our engagement, this was a good way to build a comfort level between Gaslight and Sharethrough. The downside was significant because sometimes this approach meant weeks between finishing a feature and hearing that it went into production. It also made any issues that came up with a feature difficult to address. The developers had moved onto something else quite some time ago, and context switching is the bane of almost every developer’s existence.

## The move from better to best
Over time, we built a good comfort level with the Sharethrough team, and they became much more efficient and responsive at reviewing and merging pull requests. We were reasonably happy, but only because we didn’t know what we were missing. When we had the initial meeting to talk about adopting continuous deployment, we had some concerns. Would our build process be too slow? Did we have adequate acceptance test coverage? 

Even though Gaslight already does continuous deployment on other projects, it seemed more daunting to adopt it on Sharethrough, because the project involves a large, distributed team spread across several time zones. To their credit, Sharethrough committed to removing any roadblocks that came up and making it work. If we had problems, it would mean something was wrong with our process, not our developers, and we would fix it together.

We are now several weeks into our transition. The first week or two we had a build that automatically deployed to staging. We’ve now taken off the training wheels, and every successful build goes to production. There is something incredibly empowering about the change. You can immediately make things better; there’s nothing in your way. 

It’s also a huge amount of trust, and it makes you naturally want to honor it. But even if things do go wrong, you know you can fix it right away. There’s no “Do we wait until next week's release or do a hot fix?” It’s just git push origin master.

## Encouraging the right things
Extreme programming (XP), one of the earliest agile methodologies, talks a lot about always writing production ready code. A lot of the practices, such as test driven development, are meant to enable this practice. It turns out the best way to write production ready code is to always put your code in production. If your test suite isn’t solid, you’re sure to find out! Until then, it’s too easy to write almost production ready code. Like a lot of good development practices, the way to make something easier is to do it all the time.

There’s also a lot of focus in XP on breaking down stories into the smallest thing that’s valuable. This, again, is easier said than done. If your release cycle is a couple weeks, it’s arguably easier to bundle things into larger chunks. But when every commit goes to production, you are highly motivated to break things down into the smallest possible unit of value. It’s now worth the trouble to rethink a feature, so it can be rolled out iteratively. Each tiny piece will go to production and start generating value right away. For larger features that can’t be broken down, we have feature flags to turn things on or off for a subset of users.

## But it’s scary

For projects that haven’t adopted continuous deployment, it can definitely seem like a scary thing to do. Our experience so far is that these fears are largely unfounded, and the payback is immense. It really is hard to convey the uptick in excitement and enthusiasm that we’ve seen going through this process. It turns out developers just want to ship features, and customers do, too.

