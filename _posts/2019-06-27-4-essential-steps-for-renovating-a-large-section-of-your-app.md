---
layout: post
post_author: Joel Turnbull
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 4 Essential Steps for Renovating a Large Section of Your App
publish_date: 2015-04-14 14:00:00 +0000
feature_post_image: "/uploads/renovating-v1.gif"
post_images: []
slug: 4-essential-steps-for-renovating-a-large-section-of-your-app

---
There is a section of your app that has outgrown itself. What your users need from the app has evolved. Your business is not the same as it was when the user interface (UI) was originally built. You have crammed too much functionality into one area. The multitude of options and interactions has made the code complicated. Complications have led to bugs. It takes a long time to add anything to this section of the app. Your team doesn’t like working on it. 


This is the natural way, even when you have a talented team working in a great environment. You don’t have to feel bad about old code. But you still have to fix it. Unfortunately, renovations are no small task. Redoing a section of an application can be more challenging and intense than starting something from scratch. You work within the constraints of an existing foundation. 


This whole process can seem daunting, but I’ve been following a four-step process that makes it more approachable and manageable:


## Step 1: Propose the renovation

If the app is buggy and development work is time-consuming, it won’t be hard to get buy-in from the people who can authorize you to do the work. A good starting point: a developer joining forces with a designer. The designer should understand where the UI falls down and have some ideas on how to improve it. The developer should have an understanding of why the code is complex and hard to work on. 


When I partner with [Gaslight designer Kenny Glenn](https://teamgaslight.com/people/kenny-glenn), we might head into a huddle room and start whiteboarding an improved flow or interface. I’m often surprised at what we come up with. There’s a natural back and forth. Kenny might propose a UI solution, and I’ll chime in with a slight alternative that achieves the same goal and saves hours of development time. Eventually, our goals align and we arrive at a high level plan for the renovation.


At this point, you may still be unsure about the feasibility and scope of the work, but that’s OK. Don’t be afraid to present the client with photos of rough sketches you drew on a whiteboard. Instead of emphasizing the finished product, focus your time on communicating the reasons, goals and benefits of what you’re proposing. This step can be tricky. You might not get the green light right away. Plant the seeds and be patient. Take opportunities to revisit the proposal when you can.


## Step 2: A spike is in order

Once you have buy-in to investigate your idea, request a day for [a development spike](https://teamgaslight.com/blog/mitigating-risk-through-technical-spikes) and additional time with the people who use the app. You’ll want to shop around a prototype after your spike and gather feedback.

A spike is an exercise where the developer tears into code with reckless abandon in an effort to learn something. When the exercise is completed, she throws the work away.

What you are trying to learn is:

* How does this currently work from end-to-end? 
* What sections of the code can be reused, removed or replaced?
* What is the best way to accomplish this renovation in stages?


During a spike, I like to cut a wide a swath and act like I’m starting from scratch. The last time I did a renovation we were trying to improve a multi-step flow of pages to create and configure an ad. The flow had become muddled and buggy as the business had evolved to support more and more types of advertisements. 


I started by creating a clean entry point into the section. I pulled in enough old code to get started and attempted to recreate the happiest, most direct path through the flow. Try this, and see how far you get. The process of pulling in and hooking up code will inform what code can be reused and what code needs to be removed or replaced.


Much is still unknown, but you should have a better idea of the scope and possibility of your renovation project after a spike. Share what you learned, propose some action and listen. The feedback I got from my last post-spike proposal motivated me to change my approach and spike again. Each new spike is a learning exercise and gets you closer to understanding the scope and value of the renovation.


## Step 3: Plan the work

After spiking, you may be chomping at the bit to tear the whole thing down and rebuild it from scratch. Resist that urge. Instead, propose a plan for doing the work in stages and introduce new pieces gradually. Remember, the app is some user’s baby. And while you may hate the code, never trash talk. Working on renovations has given me a new level of understanding and appreciation for code decisions made before me.

 

When you card up these user stories, don’t cut a wide swath anymore. Don’t write stories that execute a renovation end-to-end. Write stories that allow you to gradually add new pieces and gradually replace old ones. And leave yourself some wiggle room to consider, refactor and clarify. You want to write clear code for future developers. 


## Step 4: Do the work

Renovations are rife with opportunities for collaboration, so sit right next to your team while you do the work. The road forks frequently, and the direction and velocity will benefit from spontaneous team pow-wows. Renovations are also a great opportunity for pairing. You’ll be making a lot of decisions, and the instant feedback from a pair helps you navigate them quickly and confidently. 


Lately, I’ve been pairing with [Gaslight Developer Alan Audette](https://teamgaslight.com/people/alan-audette). For the most recent renovation we worked on, I had more experience with the the project, the framework, and where to find code, so I usually took the driver’s seat. Alan’s fresh take and programming experience, however, were invaluable in providing alternative solutions. 


Even though renovation was the the goal, I found myself unknowingly defaulting to pre-existing code conventions. When inconsistencies were creeping in from a conventional way of passing objects around through multiple channels, it was more obvious to Alan that there was no reason this particular process needed to be so complicated. By reflecting for a minute on what we were actually trying to accomplish, we were able to simplify it. Don't be fooled by complicated code, it promotes the assumption that the process is more convoluted than it actually is.


For this particular renovation, our approach was never to change existing code; we’d make a copy and change it instead. This allowed us to preserve the existing functionality in production, and have the new flow running in parallel for early adopters. The two flows came together where code could be shared, and branched apart again where it couldn’t. We didn’t have to write new feature specs just to cover a flow that would soon be deprecated. We were able to focus on moving forward with a new flow that was developed with excellent test coverage from the get-go. 


I recommend reusing code where you can. If you are going to touch original code as part of the renovation, make sure that you cover existing functionality with feature and integration specs, so that you can work worry-free from breaking production. It’s also crucial to keep a list of code that did not get reused. Once the renovated area goes into production and there is confidence, you can revisit the list and remove the original versions where the code branched.


It’s also key to stay focused. You’ll encounter plenty of opportunities for improvement along the way, but do not forget what you are trying to accomplish right now. As you encounter improvement opportunities, write the idea down and let it go. Once you’ve committed, revisit the list and knock them off before picking up the next step.


## Celebrate your progress

Renovations can be thankless, so take the time to acknowledge your wins. I’ve been involved in renovations and refactorings where the end user would never notice a difference but the business reaped great benefits from increased efficiency. Find a way to put a face on your renovation. Track the results, report them to the larger team and celebrate that something big has changed. After all, a successful renovation should energize the team and breathe new life into the app. 
