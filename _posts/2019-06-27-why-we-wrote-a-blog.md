---
post_author: Chris Moore
categories:
- Development
- Business
tags: []
post_title: Why We Wrote a Blog
publish_date: 2013-07-22T13:39:00.000+00:00
feature_post_image: "/uploads/blog2.png"
post_images: []
layout: post
current_gaslighter: false
slug: why-we-wrote-a-blog
permalink: "/blog/:slug"

---
We just shipped a new version of our blog. It's a [Rails app][app]. It's
not some fancy-pants, johnny-come-lately static site generator. Why on
Earth would we do such a thing?

Well, it turns out there are several reasons.

## It's Rails

We write Rails apps everyday. We write them for our clients, we write
them for our own internal use and even for prototyping. There's a lot of
value in the convention that comes with working on a Rails app.

Rails is crazy powerful. It's easy to forget. When you have a well
defined problem and want to solve it, you can move super fast with
Rails. This is it's sweet spot: rendering HTML pages and getting them to
the browser.

Static sites are useful. I won't disagree, but Rails also has a crazy
powerful toolchain built in that lots of projects are attempting to
replicate. I _know_ I can use tilt, sprockets, etc. outside of Rails,
but why? Lately I've adopted a new phrase: "give up and use Rails". It's
really nice to stop fighting against some preprocessor library and just
build things.

## Design

When the blog was hosted on Tumblr, editing the theme was basically a
nightmare. Yes, I know about Fumblr. And Thumblr. And Thimblr. And all the other
half-baked attempts at replicating the Tumblr API locally. It's just too
hard.

Our design team works in Rails apps everyday. In less than a week, we
went from a disconnected, difficult to change place to host content to
something that uses all our brand styles out of the box.

## Posting

We write our posts in Markdown and edit them on GitHub. We like the pull
request work flow for notifying others to proofread and suggest edits. Sometimes
we'll suggest something that might have been missed.

Once we're finished editing, [Joel][joel] posts the article to the blog
on his schedule. Tumblr makes that really hard, since Joel couldn't log
in and post the article as the author. We're just using ActiveAdmin, so it's stupid
simple.

## Contributing

It's now really easy for anyone on the team to suggest a change. It's
open source, after all. Make a change, submit a pull request. Bam!

## Data

We're no longer beholden to the Tumblr API, we own all of our data now.
We like to mix Google Analytics data with author data, which is notoriously missing
from the Tumblr API. Now, we can attach that and any other arbitrary data we want to!
Did this post generate a lead? Let's track that!

## Engagement

We have a ton of great content on the blog and [podcast][podcast]. I want to learn
how to make it more useful to move around and see related posts. I can
do that now. Not to mention, we can roll out and define our own search
functionality to increase that engagement. I could even do some A/B testing
with something like [Vanity][vanity], too!

## Redirection

We had a lot of content all over the place. We changed our domain name
last year, so we added [Rack::Rewrite][rewrite] to handle the old URLs
and update them to the new locations. That's a big help for Google
juice.

You can see my [half baked redirect database on GitHub][rewrite_tumblr]
in all it's glory.

## Testing

We need more, but we've got some tests around what's rendered at
different times. This is really nice. We're even testing routing old
URLs and making sure the correct responses are received so when traffic
arrives, it's sent to the new location.

[app]: https://github.com/gaslight/gaslight.co
[joel]: https://twitter.com/joelturnbull
[vanity]: http://vanity.labnotes.org/rails.html
[rewrite]: https://github.com/jtrupiano/rack-rewrite
[rewrite_tumblr]: https://github.com/gaslight/gaslight.co/blob/master/lib/rewrite.rb
[podcast]: http://gaslight.co/blog?tagged=podcast
