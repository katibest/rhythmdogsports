---
layout: post
post_author: Peter Kananen
current_gaslighter: true
categories:
- Development
- Business
tags: []
permalink: "/blog/:slug"
post_title: Why Simpler is Better for Tracking Project Health
publish_date: 2014-05-27T13:00:00.000+00:00
feature_post_image: ''
post_images: []
slug: why-simpler-is-better-for-tracking-project-health

---
As a group of software developers and designers, we talk a lot about what makes for a good product. High code quality, beautiful design, intuitive UI, and excellent test coverage are a few ways we talk about and measure the quality of a software product. But how do we characterize the quality and health of a project itself? How do we measure the effectiveness of our team, the general feeling about the project, and our relationship with the client?

We've chosen a simple way to track project health at Gaslight. We look at three different categories, and grade them subjectively with simple scores that equate to emotions quite easily. These are the categories we use to break down a project's health.

## Team Health
How well is our team working together? Do our developers and designers feel comfortable working together? Do we trust one another? Are we performing at a high level? Are we pairing effectively?

## Project Health
How is our project going technically? Are we building the right thing for the client? Are we following our process? Are we drowning in technical debt?

## Client Health
Is the client happy with our work? Are we collaborating with them well? Are we able to get feedback on a timely basis? Are they enjoyable to work with?

## Overall Health
Generally, a project's health can't be any better than its least healthful aspect. We base overall health on the lowest score of the above categories.

## Scoring
We have talked a lot about how to effectively measure success in these different areas. We've thought about using scores from Code Climate, failed builds in CI, or test coverage to determine project health. We've considered using Kanban metrics like throughput, cycle time, and lead time as part of these scores. We've even thought about tracking standup attendance from the client to measure their engagement. 

However, all of these metrics fail to capture the most important part of success in agile projects - interactions between people. For that reason, we choose to score these areas very simply and subjectively. Each of these 3 areas is either 'Great!', 'Needs Improvement', or 'Bad!'. This maps well to the Green/Yellow/Red color scheme, or to smiley/meh/sad faces. In fact, that's exactly how we capture these scores.

![dashboard scores](http://gaslight.github.io/posts/assets/images/dashboard-scores.jpg)

## Frequency
We score projects on a weekly basis. Generally, we score them at the beginning of the week, and change the scores as the week progresses. Then, at the end of the week, the final score provides a summary of the week in whole. This gives a simple data set that still allows us to see useful trends over a longer period of time.

## Effectiveness
We haven't scored projects this way long enough to get meaningful long term data. However, we've found that simple scores like this do help team members understand the state of projects throughout the company. It also provides clarity on what actions steps we can take to improve a particular project. In a future post, I'll show what this data looks like over a period of months, and we'll explore the trends that result.