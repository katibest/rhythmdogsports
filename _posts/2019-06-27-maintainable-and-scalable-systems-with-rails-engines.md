---
layout: post
post_author: Kevin Rockwood
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Maintainable and Scalable Systems with Rails Engines
publish_date: 2013-12-10T20:00:00.000+00:00
feature_post_image: "/uploads/engine.jpg"
post_images: []
slug: maintainable-and-scalable-systems-with-rails-engines

---
We recently realized that this site (the one you're [reading right now](http://gaslight.co/blog/why-we-wrote-a-blog)) was getting a little too large to remain sustainable. We noticed that large parts of the app provided completely separate fuctionality and didn't really need to communicate with each other. Maintaining these separate parts as a single monolithic application was starting to become painful so we decided to break it up using [Rails Engines](http://edgeguides.rubyonrails.org/engines.html).


* Gaslight.co (static pages, newsletter signups, contact forms)
 * BlogApp engine (posts, tags, comments)
 * TrainingApp engine (courses, chapters, instructors, registrations)

## Engines 101

A Rails Engine is a stand-alone Rails app that can be mounted by another Rails app. This is acheived largely through namespacing. Controllers and models in a Rails Engine are defined within modules of the engine's namespace. Simlarly, assets specific to the engine are accessed in namespaced directories.

Rails provides a generator for creating engines just like it does for creating a standard Rails app. Rather than `rails new myapp`, run `rails plugin new myapp`. This will create an application skeleton similar to a standard Rails app that implements the namespacing described above.

## Is an engine right for me?

Engines aren't the answer for everything, and building a rails app with engines will increase development time. The benefits are that you can maintain pieces separately. Each engine can be run in isolation having it's own layout, assets, and test suite. Furthermore, architecting an app with engines forces you to really think through your dependencies and makes you isolate things from the get go.

The difficulties:

1. **Gems**. Engines define dependancies with a .gemspec, not a Gemfile, meaning your gems need to be available on a gem server like RubyGems and can't be loaded directly from GitHub. You can always vendor a gem locally, but thats kind of a hassle. You also need to be careful to use similar versions of dependancies between engines. If you're using haml 3 in one engine, use that version everywhere.

1. **Shared assets**. Sharing assets and partials between engines is hard. I've tried to various solutions, but ended up stubbing out the dependent assets within each dummy app. For instance, the newsletter signup form in the sidebar (over there &#8594;) gets pulled in from the main application as `shared/_newsletter.erb`. In the dummy app of our blog engine, we have a blank template under `shared/_newletter.erb` so it doesn't raise a missing partial error when we run the blog in isolation.

1. **Migrations**. Scoped migrations are great and make it clear which tables go with which engine. Running `rake blog_app:install:migrations` is lame, and can lead to some nasty bugs when migrations get out of sync. Before you start with engines, read [Pivotals Post](http://pivotallabs.com/leave-your-migrations-in-your-rails-engines/) and keep your migrations separate.

The awesomeness:

1. **Reusable pieces**. Now that our blog and training apps are engines, it's easy to pull them out and move them around. Because engines are just gems, they can be run inside any rails app. Running a additional instance of our blog is as easy as putting a line in a Gemfile.

1. **Separate Tests**. As apps grow, test suites can become unwieldy and take longer to run. With engines, test suites are broken apart and run faster. Because engines are isolated, you can be assured that changes to an engine wont affect other parts of your app.

1. **Quick development**. It's easier for developers understand a single engine rather than needing to understand the entire app. Rampup time is shorter.

## Conclusion

If you feel the pain of dealing with a bloated architecture, chances are you'll have places where engines make sense. If you're comfortable with Rails, Engines are just a short mental leap. It may be somewhat painful to break out your first engine, but understanding engines means In the end, you'll have a much more maintainable and scalable systems.