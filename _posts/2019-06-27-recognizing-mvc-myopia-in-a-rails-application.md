---
layout: post
post_author: Mitch Lloyd
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Recognizing MVC Myopia in a Rails Application
publish_date: 2014-05-20T14:29:00.000+00:00
feature_post_image: ''
post_images: []
slug: recognizing-mvc-myopia-in-a-rails-application

---
## How did we get here?

Stop me if you've heard this one: "Don't put logic in your views". Although
Rails developers debate how much logic is too much for a view, most have
embraced the idea. If you dare to peak into some Wordpress code you can [see
the
alternative](https://github.com/WordPress/WordPress/blob/master/wp-login.php).
The HTML to render the login page is mixed with ideas about how users are
authenticated and how password reset emails work. There is a certain type of
cohesion to this approach ("all the login stuff") but the benefits of using an
MVC architecture seem often outweigh the benefits of this cohesion.

If you shouldn't put logic in your Rails views, then where can it go? Rails
Helpers give developers a nice place to put methods that will format strings or
spit out snippets of HTML, but they don't accommodate the code that embodies the
business logic and processes of an application.

The next obvious destination for application code is Rails controllers. If
you've never seen a controller's `create` method more than fill your text
editor's screen, then you've either been very lucky or haven't worked with many
Rails applications.  The trouble with controllers is that there is almost no
limit to what logic can live in them. We needed a mantra to save us from those
1000 line controller classes: [Skinny Controller, Fat
Model](http://weblog.jamisbuck.org/2006/10/18/skinny-controller-fat-model)
appeared on the scene.

## Now what?

"No logic in your views" and "fat controller, skinny model" will serve you well
if you're talking about database queries. That's because models are well-suited
to perform queries. But what about everything else?

* Complicated user authentication
* User authorization (e.g. who can delete this post?)
* Multi-model creation and validation with side effects
* Complex error handling when interacting with 3rd party services

The biggest problems I see in Rails application design happen because developers
think in terms of 3 class types - Views, Controllers, and Models (ActiveRecord
models to be precise). Soon they're backed into a corner and start exhibiting
the following symptoms:

* They put logic into ActiveRecord callbacks that handle different use
  cases by passing in control flags: `post.save(notify: false, loose_validation:
  true)`
* They extract logic into modules (see also "concerns" or "helpers") in an
  attempt to codify ideas and make their model classes smaller
* They use observers (deprecated in Rails 4.0) to decouple their model
  events from other logic so that they can test parts of their application
  without stubbing the world
* They heavily favor gems that hook into Rails models and controllers over
  writing their own code to implement trivial features
* They give up on logicless views and start conditionally rendering HTML based
  on intricate checks of model state

I have done all of these things and I see these issues in almost every Rails
application I work on.

#### What to do?

The journey through Rails is laden with traps (Concerns, Single Table
Inheritance, Accepts Nested Attributes) and it's creator has some pretty
obnoxious stances on TDD and some OO design guidelines. Still I'm not ready to
give up on Rails yet. Learning to pull your domain away from any framework is a
necessity for non-trivial applications.

The best article to start with for a quick win is [7 Ways to Refactor Fat
Models](http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models/).
Specifically Value Objects, Service Objects, Policy Objects and Decorators are
patterns that I use everyday when coding.

If you really want to drink from the fire hose checkout [40+ Resources For
Building Robust Rails
Applications](http://gaslight.co/blog/40-plus-resources-for-building-robust-rails-applications).
And of course there is always [Growing Object-Oriented Software Guided By
Tests](http://www.amazon.com/Growing-Object-Oriented-Software-Guided-Tests/dp/0321503627).