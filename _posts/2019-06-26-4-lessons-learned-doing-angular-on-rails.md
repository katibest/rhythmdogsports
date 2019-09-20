---
post_author: Chris Nelson
categories:
- Development
tags: []
post_title: 4 Lessons Learned Doing Angular on Rails
publish_date: 2013-08-05T13:44:00.000+00:00
feature_post_image: "/uploads/angulAR (1).png"
post_images: []
layout: post
current_gaslighter: true
slug: 4-lessons-learned-doing-angular-on-rails
permalink: "/blog/:slug"

---
We've been working on one of our first Angular projects with a Rails backend. It's been a great experience. I wanted to share a few things we learned that we hope are helpful to others building Angular on Rails apps.

Skinny controllers in Angular
-----------------------------

In the Rails world, "Fat models, skinny controllers" has been some of the most oft-quoted design advice for many years. In angular.js, this also turns out to be solid advice. Getting logic out of your controllers makes it easier to reuse and also helps  improve the design of your codebase (e.g. Single Responsibility Principle). I'd like to share a couple of the ways to put your angular.js controllers on a diet.

Services
--------

The most common way you'll run across to move code out of your controller is to move into a service created by a factory and then inject it where you need it. To illustrate, here's a controller with a method that generates a random number.

````coffeescript
App.controller 'RandomCtrl', ($scope)->
  $scope.random = 0

  $scope.randomize = (max)->
    Math.floor(Math.random() * (max + 1))
````

Moving this controller method out into a service is really easy. We just define a service and then let angular inject it into our service like so:

````coffeescript
App.service "randomizer", ->
  randomize: (max)->
    Math.floor(Math.random() * (max + 1))

App.controller 'RandomCtrl', ($scope, randomizer)->
  $scope.randomizer = randomizer
  $scope.random = 0
````

This is a great way to share code between multiple controllers (or anything else in your app). Services are singletons, it's worth pointing out, so not the best place to put logic that's belongs to something you want multiple instances of.

Angular models as Coffeescript classes
--------------------------------------

In a client side framework the model layer doesn't tend to end up with as much code as server side models do. There are certain concerns (authorization, certain kinds of validation) that are always going to need to happen on the server side. But that doesn't mean that your models have to be totally anemic either. There are definitely cases where defining functions on your models is totally the right place for code to live.

In Angular, to have our models easily persist to our RESTful Rails backend, we really like a gem called [angular-rails-resource](https://github.com/FineLinePrototyping/angularjs-rails-resource). It's got some nice improvements over [ng-resource](http://docs.angularjs.org/api/ngResource.$resource) when integrating with Rails. Like ng-resource, you define your models as Angular factories. Here's an example of what a typical model factory might look like:

````coffeescript
angular.module('App').factory 'Car', ['railsResourceFactory', (railsResourceFactory) ->
  railsResourceFactory
    url: "/cars"
    name: 'car'
]
````

The end result of all this is a Car "class" you can use to create new instances. However, it wasn't at all obvious to us initially where to put methods on Car. It turns out to be easier than we thought. We can make a coffeescript class that extends from the class function that railsResourceFactory creates. Since we then have a plain ole coffeescript class, we can add methods to it just like any other class. Our factory just returns the coffeeecript class. This is what it looks like:

````coffeescript
angular.module('App').factory 'Car', ['railsResourceFactory', (railsResourceFactory) ->

  CarResource = railsResourceFactory
    url: "/cars"
    name: 'car'

  class Car extends CarResource

    drive: ->
      @mileage += 1
]
````

Putting your templates on the asset pipeline
--------------------------------------------

When you first start out with angular, you may not need to put templates in separate files at all. But as soon as you start using the router or building custom directives you'll end up needing them. The canonical way to load templates is to use the templateUrl attribute. This works fine, and you can put html files in your rails app in assets/templates and rails will serve them up no problem. For instance, if you have foo.html file in assets templates you could route to it like so:

````coffeescript
  $routeProvider.when "/foo",
    templateUrl: "/assets/foo.html"
````

It's worth pointing out that you can write your templates in haml as well, but you need do just a little extra work to make this happen. Here's the secret sauce you'll need to put in an initializer file to have the asset pipeline compile haml:

````ruby
Rails.application.assets.register_engine '.haml', Tilt::HamlTemplate
````

This lets your write your template in haml, you'll need to name it foo.html.haml in this case.

This works fine, but there's another undocumented (AFAIK) way you can give templates to angular: you can use let the asset pipeline precompile templates using a client side templating library and put them on the global JST variable. It may seem like a strange thing to do, but for us it solved a specific problem that didn't seem to have an immediately obvious solution: loading directive templates in our unit tests. We use haml to write our views in rails and had already been using hamlc (Haml in coffeescript) to do client side views. So it worked pretty well to move our templates underneath app/assets/javascripts and rename them to *.jst.hamlc. In the example above, let's suppose we moved our template to app/assets/javascripts/angular/templates/foo.jst.hamlc. If we change our route like so:

````coffeescript
  $routeProvider.when "/foo",
    template: JST["angular/templates/foo"]
````

Everything works as before.

To mangle or not to mangle
--------------------------

EDIT: This only works for Rails 4 apps.

There are two different syntaxes for telling angular what dependencies you need injected. In order to be consistently inconsistent, I've used both in this post. One is to define an array where the first arguments are the names of the dependencies and the last is the function. The Car examples above use this more verbose syntax. The other, more concise syntax is to define your function with parameter names that map to services or factories and tell angular essentially, "figure it out for me wouldya." The randomizer examples show how this looks.

But this breaks down if you use a javascript minifier that mangles (or renames) your variables. This is the default for Rails in production environments, but it's easy to turn off. Go into your production.rb file and add this line:

````ruby
config.assets.js_compressor = Uglifier.new(mangle: false)
````

And voila, at the cost of a few more characters in your minified javascript, you can use the more concise syntax.
