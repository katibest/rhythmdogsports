---
post_author: Chris Nelson
categories:
- Development
tags: []
post_title: 'Angular: The Component Orientedness It Makes Me Happy And Why'
publish_date: 2013-07-19T04:00:00.000+00:00
feature_post_image: ''
post_images: []
current_gaslighter: true
layout: post
slug: angular-the-component-orientedness-it-makes-me-happy-and-why
permalink: "/blog/:slug"

---
Being a geezer programmer ain't all bad. True, sometimes it's
incredibly annoying to those around us: "Back in my day we had a trash 80, a
cassette player, and some duct tape AND WE LIKED IT!". But other times, we get
to look back over decades of programming with some valuable perspective.

I've been doing web development in some form since about the time there was a
web, let's say mid to early nineties. I've recounted all the phases we went
through in other posts and I won't bore you with that now. What I'd like to
focus on today is a particular dichotomy in server side development: MVC vs component oriented frameworks. I
felt a very strong echo from this in my recent experience with angular lately.

First, I want to describe very briefly the examples of each type of server
side framework that I had the most experience with at the time: Struts and
Tapestry. Struts was the big gorilla at the time (sadly, it's likely still in
widespread use). Struts was classic server side MVC: it was all about mapping
URLs to java controller classes at it's heart. It was certainly possible to
build complicated web apps this way, but it was not a lot of fun. The best
description I ever heard was that it felt like "sewing with buttered boxing
gloves".

Tapestry, on the other hand, takes a completely different approach. Rather
than being chiefly concerned with mapping urls to actions, Tapestry is all
about modeling a web page as a nested structure of components (hence the term
component-oriented). Text fields, radio buttons, anything from a simple span
of dynamic text to as complex as a sorted and paginated table, are represented
as server side components. Components can contain and re-use other components.
It was really easy (and fun!) to make your own components. For a server side
framework in the mid-2000s this was really radical. Building interactive web
applications rather than page-oriented web sites felt incredibly more
productive. The awkward dance of URLs and parameters and http requests
was abstracted away (with great effort on Tapestry's part mind you) and you
were free to focus on the bits of your app that mattered. Incredibly powerful
high-level components evolved that made it that much easier.

Tapestry also takes what at the time was a novel approach to templating:
instead of embedding java code in your web page a la jsp, you marked which
sections of the page should be handled by a tapestry component by adding
attributes to your html. This made the process of integrating design and
development really enjoyable. Instead of having to turn a designer's original
html into java method calls or taglibs, you could work directly with the
original markup. You simply added a few attributes here and there and let your
designer know what they were doing and why they were needed. If there were
tweaks, the designer could continue to make changes to the html without a
painful reconversion process. It was some of the most fun I've ever had doing
web development.

What got me reminiscing about all of this was some experiences working with
Angular recently. If you've worked with Angular my description of Tapestry is
already sounding very familiar, in particular the similarity between Tapestry
components and angular directives. Angular directives let you add
application behavior via html elements, attributes and css classes. There are a
large set of built in directives that you'll end up using as soon as you start working with Angular. This also means there is not separate template language: the html/css we build
initially doesn't have to be translated into some other template language by
developers. You can work with and continue to iterate on the original static html and css that you started the project with.

The component approach of angular also has also resulted in a similarly
ecosystem of reusable components. Take a look at the angular-ui project to see what I mean. Though I've heard developers scoff at the
idea of a component library being needed, as a wise friend once remarked:
"No code is faster than no code". Though he was talking about performance, it
applies to development as well: nothing is faster to develop that something I
don't have to write at all. My experience so far working with angular
directives has been very productive.

I'm still pretty new to angular, so it may be I'm in that "honeymoon period"
where everything is new and bright and shiny. But so far the experience has
really been enjoyable. I'm looking forward to more.
