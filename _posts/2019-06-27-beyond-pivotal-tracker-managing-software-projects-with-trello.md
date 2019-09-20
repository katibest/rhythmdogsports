---
layout: post
post_author: Peter Kananen
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'Beyond Pivotal Tracker: Managing Software Projects With Trello'
publish_date: 2015-02-20 05:00:00 +0000
feature_post_image: "/uploads/kanban-1.png"
post_images: []
slug: beyond-pivotal-tracker-managing-software-projects-with-trello

---
Two years ago, we wrote a popular blog post called [“Why Not Pivotal Tracker?”](https://teamgaslight.com/blog/why-not-pivotal-tracker) It outlined some of our frustrations with Pivotal and mentioned that we hoped to replace this tool with some form of kanban. We’re often asked for an update on how we’re managing software projects now. The quick answer? [Trello.](http://trello.com)

Trello’s strength is its ultimate flexibility. A board can be modified easily to match your changing process. It doesn’t attempt to prescribe aspects of your process in the way that Pivotal attempts to do. Of course, this flexibility comes with some tradeoffs. Our biggest pain point is the lack of metrics provided by Trello. Although some of these metrics might be slightly difficult to calculate in such an open system, it seems that Trello just isn’t interested in providing them. Big sad face!!

Our preferred starting point to begin working on a project usually comes out of the story mapping process. This process is worthy of a blog post by itself, but the short of it is that we are pretty much in love with the book [User Story Mapping by Jeff Patton.](http://shop.oreilly.com/product/0636920033851.do) Story mapping really works! Trello works fairly well as a canvas for a story map. Although we usually use physical sticky notes, we often replicate the story map in Trello afterwards. Here’s a screenshot of a story map we created for an auction site that we built recently.

[![Story Map](https://gaslight-blog.s3.amazonaws.com/beyond-pivotal-tracker-managing-software-projects-with-trello/story_map.jpg)](https://gaslight-blog.s3.amazonaws.com/beyond-pivotal-tracker-managing-software-projects-with-trello/story_map.jpg)

At this point, we start release planning using the principles explained by Jeff Patton and pull a horizontal slice from the story map. This stack of stories represents a chunk of stories that we want to work on next. But we need a new Trello board to track things we’re building! At this point, we’ll follow the kanban best practice of modeling our work in columns. Here’s what we usually start with: Up Next, Design, Development, Testing/Acceptance and Done. This is our basic set-up, but we’ll change it slightly based on a project’s needs. Sometimes we have a column related to QA work, or the work of an Accessibility team. To populate the build board, we take our stack of stories we’ve selected from the story map and put them in Up Next.
 
Below is a sample build board for that same auction site project. Note that we use weekly dated Done columns, so it’s easy to see what we accomplished that week. The numbers in each list name are work in progress (WIP) limits. Trello doesn’t enforce WIP limits, but there is a Chrome plugin that will highlight columns in red or yellow.

[![Build Board](https://gaslight-blog.s3.amazonaws.com/beyond-pivotal-tracker-managing-software-projects-with-trello/build_board.jpg)](https://gaslight-blog.s3.amazonaws.com/beyond-pivotal-tracker-managing-software-projects-with-trello/build_board.jpg)

As a project matures and the original story map has served its purpose, we’ll create a Planning board. This usually happens when we start to get a lot of feedback from real world use and people discover bugs as we’re still working off of our story map backlog. The planning board serves as a place to triage incoming feature requests and bugs and to flesh out stories. Stories that we decide to work on end up in the Ready for Up Next list on the Planning board and are ready to be pulled into the Up Next column on the Build board. 

[![Planning Board](https://gaslight-blog.s3.amazonaws.com/beyond-pivotal-tracker-managing-software-projects-with-trello/planning_board.jpg)](https://gaslight-blog.s3.amazonaws.com/beyond-pivotal-tracker-managing-software-projects-with-trello/planning_board.jpg)

Occasionally, we’ll also create an Engineering board. It’s for the developers to self-manage tasks that are not scheduled stories. A team tackles these projects as they have the time or desire, and occasionally, they’ll be completed as part of a user-facing story. Here’s another example from our auction site project.

[![Engineering Board](https://gaslight-blog.s3.amazonaws.com/beyond-pivotal-tracker-managing-software-projects-with-trello/engineering_board.jpg)](https://gaslight-blog.s3.amazonaws.com/beyond-pivotal-tracker-managing-software-projects-with-trello/engineering_board.jpg)

So why do we like Trello? It’s a spot where our client product owners and development teams can keep track of what’s happening together. We believe a high-functioning development team can work directly with product owners, and Trello helps supports this close collaboration. To keep designers and developers accountable, we focus on low WIP, high throughput and any bottlenecks that reduce the flow of work.
