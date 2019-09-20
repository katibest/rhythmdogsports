---
layout: post
post_author: Michael Guterl
current_gaslighter: false
categories:
- Business
tags: []
permalink: "/blog/:slug"
post_title: How We Trello
publish_date: 2014-01-17T15:00:00.000+00:00
feature_post_image: ''
post_images: []
slug: how-we-trello

---
We started using Trello on one of our projects because we wanted better
visualization of our process and more flexibility than Pivotal Tracker could
provide. Pivotal Tracker provides three standard columns and five standard card states:

* Icebox
* Backlog
* Done

* Started
* Finished
* Delivered
* Accepted
* Rejected

This workflow [no longer fit with our process](http://gaslight.co/blog/why-not-pivotal-tracker) as we did more work to
integrate design and development. We considered creating multiple cards for each story,
but this caused confusion.

Pivotal Tracker also lacked the concept of WIP limits. Trello doesn't support
WIP limits, however, in Trello it's much easier to see when there is a WIP violation. There is
a also a [Chrome extension](https://chrome.google.com/webstore/detail/kanban-wip-for-trello/oekefjibcnongmmmmkdiofgeppfkmdii?hl=en-US)
that can help visualize WIP.

We're trying to model our current process with Kanban, using the
following columns and WIP (work in progress) limits in brackets: 

![wip](http://gaslight.github.io/posts/assets/images/how-we-trello_wip.png)

## Columns 

* Up Next [4] 
* UX / UED [2] 
* Development [2] 
* Staging / Acceptance [2] 
* Done (Production 1.0.0) 
* Done (Next Version 1.1.0) 

![big picture](http://gaslight.github.io/posts/assets/images/how-we-trello_big-picture.png)

## Labels 

Trello also provides labels and we're using different colors to represent different states:

* Green: Ready to Pull
* Blue: Expedite (should only be one story at a time with this color) 
* Red: Bug
* Orange: Not Accepted
* Yellow: Blocked

![labels](http://gaslight.github.io/posts/assets/images/how-we-trello_labels.png)

Trello is flexible and simple. Take whatever process you're currently using and
translate it to a Trello board. As you realize that you're no longer bound by
the prescribed method of <project management tool> you may find a better way to
represent how your features move from idea to completion.

Besides managing our client projects we use Trello for all different
types of things:

* Recruiting
* New Hire Onboarding
* Organizing User Groups
* Book Club
* Blogging

Our current client has enjoyed the workflow and simplicity
of Trello and he is now using it for his internal projects as well.