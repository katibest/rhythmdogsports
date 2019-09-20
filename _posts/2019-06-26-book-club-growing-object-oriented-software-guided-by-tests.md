---
post_author: Joel Turnbull
categories: []
tags: []
post_title: 'Book Club: Growing Object Oriented Software Guided By Tests'
publish_date: 2013-08-26T14:15:00.000+00:00
feature_post_image: "/uploads/cover.jpg"
post_images: []
layout: post
current_gaslighter: false
slug: book-club-growing-object-oriented-software-guided-by-tests
permalink: "/blog/:slug"

---
**Book** [Growing Object Oriented Software Guided By Tests](http://www.amazon.com/Growing-Object-Oriented-Software-Guided-Tests/dp/0321503627)

**Chapters** 1-4

**Participants**

mguterl, cdmwebs, pkananen, too_mitch, dougalcorn, joelturnbull, st23am, agilous, kevinrockwood, superchris, heflinao

---

We recently started reading and discussing GOOS as a group. Here's some high points of the first get together.

First, we talked about ubiquitous language. A lot of times we talk about refactoring, when we're really talking about a straight up redesign. It's time to get our definitions down so that we can communicate more effectively. One of the worst offenders is talking about mocks when we're really talking about stubs, or vice-versa. A mock tests an expectation of a method call, a stub can start with an expectation but is really a tool for setting a return a value.

We began to apply definitions to kinds of tests: Acceptance, Integration, Unit. This led to a discussion of how Unit Tests of ActiveRecord models aren't actually that. Because tests of ActiveRecord models actually test the interaction of your objects with database adapter code that can't change, we're not really testing an object in isolation. Someone suggested the term Unit-Integration test for this.

This led to a deeper discussion. Is this book really about Rails? Many were of of the opinion that GOOS applies less to what we traditionally think of as "Rails" objects (objects that are things) and more to Ruby objects that emerge as we break out of the framework (objects that collaborate do things). It was stated that this breakout should occur as soon as possible in a Rails project. We also discussed some patterns for decoupling the application from the database.

One of the guiding principles for design that came from this discussion was that we should hold our own code to the same standards to which we hold the libraries that we use.

We spoke at length about getters and setters. I'm not sure any conclusions were reached there. One of the revelations of GOOS is the focus on testing the message and not the state, using mocks. Testing state presumes exposing state, which breaks encapsulation. A design that is privy to the internal state of objects makes our system rigid.

Discussions on good design led to a discussion on code authorship. We all start life as developers focused on just making things work. Many of us are experienced enough to have been confused in hindsight by our own code written in this way. How can we better inform our future selves?

One way is just to be intentional about it. Structure a class as a story we want to tell ourselves in the future. Add methods that describe intent. Make use of "private". Write tests that describe the interface.

In fact, a takeaway from the discussion is that tests can be one of the best ways to tell the story. They can more perfectly describe exactly what the system is expected to do. Most agreed that, being dropped into a codebase, poorly written code was preferable to poorly written tests.

Stubbing can eliminate excessive setup in tests. However, in acceptance tests a lot of setup can't and shouldn't be avoided.

We ended with some related concepts. Coupling is the circumstance in which a change in one object forces a change in another. Cohesion relates to a balance in responsibility. An object can have too many responsibilities. An object's responsibilities can be broken out to the point where the object doesn't represent a full concept. Neither object is cohesive.
