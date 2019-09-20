---
layout: post
post_author: Alex Padgett
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: How Good Developers Avoid Bad Code
publish_date: 2014-12-04 15:05:00 +0000
feature_post_image: "/uploads/trade-off-v1.gif"
post_images: []
slug: how-good-developers-avoid-bad-code

---
Developers often like to think of themselves as engineers. We value things that make sense above all.  Code is no different.  Principles and rules often heavily influence the decisions we make while coding.  With all of the information surrounding us, sometimes it can feel like our decisions can be heavily guided or even be made for us.

Yet in every project there is always some trade-off to be made.  Situations where there is no single correct answer to a given problem.  As developers, the best that we can do is to weigh the pros and cons and make the best decision with what we have and hope we don't end up with the dreaded "bad code."

Writing code is like writing an English paper.  If you come back in six months, you're almost always disgusted with what you wrote.  It can be even worse when looking at someone else's code.  It always seems like there was some trade-off that went the wrong way.

In the spirit of making these decisions more concrete, there are countless lists of design patterns, object-oriented principles, and dos and don'ts.  It is advantageous to follow some sort of formula that helps you prioritize one trade-off over another. Below is a priority queue that I use to help guide decisions when I'm coding.

## Simplicity
Realistically, developers write good code to save our clients and future selves time, money and headaches.  Often times developers try to get too elegant or "tricky" with their code.  This only creates confusion down the road for anyone else that has to understand what's going on.  The best code is code that any competent developer can sit down with and understand within a few minutes.

## Brevity
This goes hand in hand with simplicity.  Write as few lines as you possibly can without sacrificing the simplicity of the code.  All too often developers put their heads down and come to an hour later with multiple 20+ line methods.  Large chunks of code are often a sign of complexity that should be reduced to smaller, more concise code.

## Readability
Building again on the concept of keeping code simple and concise, keeping code readable is very important.  This is the difference between writing a full if/else statement and using the ternary operator when it makes more sense.  Languages like Ruby and Coffeescript lend themselves more than others. Everyone reads from left to right, top to bottom. The closer your code reads to a sentence the easier it will for other developers to consume.  It should be fairly easy to accomplish this without sacrificing simplicity or brevity.

## Modularity

Modularity is probably the most inline with traditional object-oriented design principles of this list.  Code that is logically separated and grouped together is important for discovery within the code base as well as breaking complex concepts into smaller, more concise code.  It's important to realize that simplicity and brevity take precedence over this, and knowing when the right time to break something out into it's own module is.  Referencing things like the Single Responsibility/Interface Segregation principles from [SOLID](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design)) can help with determine if it's the right time.

## Reusability
Some developers often try to solve the problems of the future and as a result end up with a mess of code.  This is likely due to violating multiple of the principles listed above.  Reusable code is important, but only use it where it fits.  In most cases, [you aren't gonna need it](http://en.wikipedia.org/wiki/You_aren't_gonna_need_it).  Don't try to jam everything into one method to keep it reusable; it directly conflicts with simplicity and readability.  Solve the problem at hand, refactor areas that you see clear improvements, and address the your future problems when they get happen.  Some may argue that not looking into the future can come back to haunt you, but in most of these cases this is likely because the existing solutions were not simple, brief, readable and modular.

## Gut check
This one is a bit of an intangible but it's important to listen to your gut.  If you look at your code and you say to yourself, "There *has* to be a better/easier way of doing this.", there almost always is.  Time permitted, look around on the internet or talk to other developers and find other approaches.  Most good developers can sense when they're doing something dirty and try to fight against that feeling.

There are obviously other variables to writing code, and this list is not the answer to all of your problems or questions.  But it is a good start.  Hopefully it serves you as well as it has served me so far.
