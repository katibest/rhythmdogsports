---
post_author: Joel Turnbull
categories:
- Development
tags: []
post_title: Why I Chose AngularJS
publish_date: 2013-06-28T17:31:00.000+00:00
feature_post_image: "/uploads/angular.png"
post_images: []
layout: post
current_gaslighter: false
slug: why-i-chose-angularjs
permalink: "/blog/:slug"

---
I chose [AngularJS](http://angularjs.org/) because it seemed closer to what I know.
More like Backbone. [Data-binding](http://docs.angularjs.org/guide/dev_guide.templates.databinding)
is touted as Angular's killer feature over Backbone, but there's a lot more to
Angular. I also chose Angular because everybody else here in the shop was
learning Ember. What is wrong with me?

### Views

Angular templates are HTML. I know HTML, that side of Angular gives me comfort.
You write valid HTML and "sprinkle" it with Angular [directives](http://docs.angularjs.org/guide/directive)
(ng-repeat, ng-hide, etc). Angular traverses the DOM, collects the directives, and produces
a live view that is plugged into your application's scope. It's cool. You can
write directives to customize your UI.

### Scopes

Angular [scopes](http://docs.angularjs.org/guide/concepts#scope) are something you encounter quickly
and it's an important piece. Scope is actually $scope in an Angular app, and it is the
connection between an Angular controller and the view. Every object or function
you attach to $scope in the controller is available to the view. Scopes model
the DOM and are hierarchical.

### Models

I was intrigued by the lack of an Angular Model. I've always liked the
flexibility and simplicity of Javascript objects. Models in Angular are any object,
simple or complicated, that you choose to attach to the app's scope.

But modeling the relationships between objects in my app wasn't simple. When I
have a Post that has an Author, and Author that has many Posts, and no
"database", things get trickier. This isn't Angular specific, and probably
speaks to my inexperience a bit. I had to be very diligent about maintaining a
single source of truth, and eliminating state. It feels like this would be easy
in Ember.

### Services

The Data problem is the arena of Angular [services](http://docs.angularjs.org/guide/dev_guide.services.creating_services). You write Angular services to
share things between controllers in your app. In my case it was a collection of
Post models and a collection of Author models. That was my data and every
controller in my app needed access to it. You hear a lot about dependency
injection. Services is where that comes into play. When you define a
controller you also define what services it's going to need, and pass those in.

### Documentation

Documentation is really good in Angular. In fact I'm sitting here reading it and
realizing I haven't even scratched the surface of how cool this is. Here are
some of my favorite pages:

- [Overview of Concepts](http://docs.angularjs.org/guide/concepts#view)
- [API](http://docs.angularjs.org/api)
- [Examples](http://angularjs.org/)

### Conclusion

There's not a huge learning curve for Angular. There are a few unfamiliar
concepts that you run into early, and can read about. The "right way" to do
things isn't apparent at first, but it's up to you and you can go fast. That can
be a double edged rope with which to hang yourself. Writing an Angular app was
enjoyable and painless, and Angular does some cool simple things that make sense.

Thanks to [Michael Ball](http://twitter.com/ballmw) for giving me some exposure
to this awesome tool.
