---
post_author: Chris Moore
categories:
- Design
tags: []
post_title: Why I Chose Zurb Foundation
publish_date: 2013-07-26T19:50:00.000+00:00
feature_post_image: "/uploads/zurb.png"
post_images: []
layout: post
current_gaslighter: false
slug: why-i-chose-zurb-foundation
permalink: "/blog/:slug"

---
We use [Zurb's Foundation][zurb] framework on all our projects at Gaslight. I've been
meaning to talk about why I believe Foundation is superior to Bootstrap for
some time and what better time than today.

## Mobile First

Foundation has been [mobile first][mobile] since the beginning. Responsive sites are
pretty important to me, since I'm pretty mobile. I'm a fan of the grid and the
ability to change the columns between screen size with little hassle.

## The Grid

Foundation's grid is (was?) unique in that I can embed a row within a column
and it allows me to use the same twelve columns again. I don't have to keep
track of the current row and column counts when I'm prototyping. It's really
nice to split the page in to six columns, then dive in to a four/eight split
inside each.

![nested grid](http://gaslight.github.io/posts/assets/images/nested-grid.jpg)

## Unstyled, by default

With Bootstrap, you can add an `unstyled` class to elements to remove the
opinionated style. I prefer Foundation's approach here. Zurb is a design shop,
so Foundation is pretty bare in terms of the style that "bleeds through". If
you're using Bootstrap, you're immediately another Bootstrap site.

I like the baseline that Foundation provides our designers. It's name is really
descriptive of it's purpose, as I see it. Sensible defaults are one of my favorite
things.

## Sass and Rails

We're a Rails shop. We work with Ruby on Rails everyday, all day long. Everyone
here knows how to clone an app, run `script/setup` and get started.

We use Sass, too. Sass and Rails work great together.

Foundation is packaged as a Rails gem by Zurb. It's written in Sass. It's a no
brainer. I don't have to wait for ports from Less. It works with the asset
pipeline. As they often say, it "just works."

## Modularity

Since Foundation's written in Sass, I can pull in the pieces I want as needed.
Only using the grid? `@import foundation/components/grid`. Done. How awesome is
that?

If I want to override the column widths, that's a piece of cake, too. I can
override `$row-width` before I pull in the grid.

## Components

Foundation ships with some handy components out of the box. We've used Joyride,
Sections and Tooltips pretty frequently.

## Constant Development

Foundation is under almost constant development. It's updated frequently. That's
exciting for me. They've built something great and it keeps getting better with
every release. There's a large and active community participating in
development as well. As of this writing, the [foundation repo][repo] has 12,621
stars. That's pretty impressive.

## But, semantics!!

I know, it's terrible. I have classes all over my HTML that aren't part of the
content. There are classes like `large-6` and `small-centered` on elements.

I don't care anymore.

For a long time, I worried about the CSS police. I was certain that everyone
who inspected my page would immediately run away screaming and refuse to listen
to my message or buy the product. I was wrong and I wasted a lot of time
because of that choice.

Shipping something is the most important thing. If I delay that because I need
to refactor my stylesheets, my priorities are in the wrong place. This is an
example of when craftsmanship gets in the way of productivity, in my opinion.

## Conclusion

I'm sure you'll disagree. Maybe you won't, but that's okay, too. Bootstrap is
great for a developer who wants to ship something. I'm glad there are tools
that help you put just enough polish on an idea and get it out the door. That's
better than discussing the importance of understanding floats, in my humble
opinion.

That's a topic for another day.

[repo]: https://github.com/zurb/foundation
[zurb]: http://foundation.zurb.com/
[mobile]: http://foundation.zurb.com/mobile.php
