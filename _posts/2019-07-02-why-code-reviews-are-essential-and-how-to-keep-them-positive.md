---
layout: post
post_author: Tyler Shipe
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Why Code Reviews Are Essential and How to Keep Them Positive
publish_date: 2016-04-07 12:00:00 +0000
feature_post_image: ''
post_images: []
slug: why-code-reviews-are-essential-and-how-to-keep-them-positive

---
For the past three months at Gaslight, I’ve been doing regular code reviews with fellow developer [Alex Heflin](https://teamgaslight.com/people/alex-heflin) (known as Super Alex around the office). We’ve fallen into a really good rhythm where we use a combination of GitHub pull requests and in-person code reviews around a screen. These sessions have been a great method for gaining not only technical knowledge, but as a new employee, process and cultural knowledge, too.

Unfortunately, not every code review is created equal. In the past, I have experienced code reviews that have been both a positive and negative experience, and this mixed bag seems to be common among developers. Each team and individual will benefit from code reviews differently, but I believe there is always value to doing them. The key is keeping them positive for everyone by understanding the goals of a code review and working on our soft skills. 

## Boosting Code Quality
Improving code quality may be the most obvious result of code reviews, and it is certainly beneficial in that way for me. Not only do reviews have an immediate benefit on the code you're reviewing, but you also gain new insights into solving problems that you can apply to future projects. These benefits were the original motivation behind Alex and I doing code reviews with Gaslight [Chief Scientist Chris Nelson](https://teamgaslight.com/people/chris-nelson), and eventually, conducting reviews with just each other. 

As an additional  benefit, I found that just knowing I was going to be code reviewed led me to write better code. I wanted to predict questions I might be asked in a code review and think through how I would answer them. This often changed how I looked at my own code.

Alex and I both experienced this in our code reviews with Chris while working on an internal scheduling application. Because Chris wasn’t very familiar with the application, we found this lack of familiarity encouraged us to write even cleaner code that was well tested and easier to understand so that it could be reviewed in isolation.

## Building Shared Understanding
In a team environment, gaining a shared understanding and discussing the context of the code change should be a priority. Additional background information will often be exposed that the developer didn’t know about. 

Also, the original developer of a feature is rarely the only person who will maintain it. This shared understanding about why the code is changing makes it easier for other developers to jump in when needed.

While working on the internal app, we often pulled our [Delivery Manager Peter Kananen](https://teamgaslight.com/people/peter-kananen) into our code reviews. He is one of the primary users of the app and did a lot of the original development. This proved helpful as we made changes that automated Gaslight’s invoicing process. During code reviews, Peter was able to give background information around some complex date logic that we would have missed otherwise.

## Improving Soft Skills
Negative experiences with code reviews often stem from the interpersonal aspects. Sometimes feedback is well intentioned but not well phrased. This is even more apparent when reviews are done online (i.e. GitHub pull requests) due to a negativity bias in written communication. In person communication can at least be controlled by tone and expression.

Thoughtbot has published a [nice guide for reviewing code](https://github.com/tjshipe/guides/tree/master/code-review) and having your code reviewed that I suggest everyone check out. I’ve highlighted some of my favorite takeaways below:

Ask questions; don't make demands: “What do you think about naming this :user_id?”

Avoid selective ownership of code: mine, not mine, yours.

Don't take it personally. The review is of the code, not you.

Communicate which ideas you feel strongly about and those you don’t.

Be humble: “I'm not sure. Let's look it up.”

## Setting a Time Box
Code reviews take time and tight deadlines are often used as an excuse to not do them. However, I would argue that tight deadlines means there isn’t time to not do a code review.

Agreeing on the scope and maximum time frame for a review session will help prevent in person discussions from turning into an unproductive debate. If discussions turn too philosophical or academic, encourage the discussion to be continued outside of the code review. In the meantime, let the author make the final decision on alternative implementations. 

I’ve enjoyed my past three months of code reviews at Gaslight and look forward to continuing them as I move to other projects and work with other developers. They are key for building great software and continuing to improve as a developer. 
