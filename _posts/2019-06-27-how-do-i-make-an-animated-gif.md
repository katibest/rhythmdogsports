---
post_author: Joel Turnbull
categories:
- Development
- Design
tags: []
post_title: How Do I Make an Animated .gif?
publish_date: 2012-08-09T14:57:00.000+00:00
layout: post
current_gaslighter: false
feature_post_image: ''
post_images: []
slug: how-do-i-make-an-animated-gif
permalink: "/blog/:slug"

---
Here's an animated gif of a certain sporting event that I saw on a certain
broadcasting network's website tonight. I expect a cease and desist any minute
now, so let's hurry.

![](http://twitpic.com/show/thumb/ah4i7m.gif)

I made that (President Obama), but I don't know if I would do it again, unless
it was easier. So what's the best way to create an animated gif with mostly
free tools, or highly-recommended cheap tools, on OS X?

I can let you know how I did it, but I can promise you it's horrible.

  1. I used Quicktime Player's File -> "New Screen Recording" to record the clip, playing on the site. Win.
  2. I trimmed the clip in Quicktime, and attempted to upload it to several GIF-making websites. Failure all.
  3. I opened the clip full screen. I mashed the right arrow key four times and then took a screenshot, and repeated until the scrubber hit the end of the clip. 58 times. Work harder, not smarter.

![](http://media.tumblr.com/tumblr_m8h0b5pPnz1r9fv8b.png)
![](http://media.tumblr.com/tumblr_m8gzzk6Qef1r9fv8b.png)

  4. I needed to compile these into a .gif image. Pixelmator couldn't do it, Preview could not hang either, Pixen kept crashing. I landed on, of all things, [GIFfun](http://www.stone.com/GIFfun/). Congratulations GIFfun, you have been selected!
  5. I created a workflow in OS X Automator to scale the 58 images down 33%, because screenshot-ing them all at full screen resolution (see Step 3) was a terrible idea. I don't hear many people talk about Automator. If you need to automate stuff, Automator is the best.
  6. GIFfun took a little finagling at first. It's either a little wonky, or not 100% idiot proof. There was a window that I closed, and couldn't get back without re-opening the app. But anyway, "Load Folder…" turns out to be what I wanted. It chronologized the images in a timeline with a default delay value, and when I clicked "Make Gif", it was damn near instantaneous. I have to hand it to GIFfun on that.

So that's my long story. I forgot to mention that I managed to record the
Quicktime Player controls in every screenshot, which is lame. I don't see a
way around that, except to not do it that way.

So what is the best way?
