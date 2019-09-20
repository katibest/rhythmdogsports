---
post_author: Mitch Lloyd
categories:
- Development
tags: []
post_title: Intermediate Ember Controller Concepts
publish_date: 2013-07-03T10:24:00.000+00:00
feature_post_image: "/uploads/ember-1.png"
post_images: []
layout: post
current_gaslighter: false
slug: intermediate-ember-controller-concepts
permalink: "/blog/:slug"

---
> This post was updated on 2/18/2014 to reflect changes in Ember and incorporate
> feedback from some of the comments.

I've been finding issues in an Ember application I'm working on where the author
(me) didn't fully grasp Ember controllers. Here are a few controller concepts
I've learned.

Controller Concepts
-------------------

* A controller is a decorator for your models. It proxies all
  the `get` and `set` calls to the underlying model unless that property is
  defined on the controller itself.

* Singleton controllers are long-lived objects that hold onto application
  state. Models change beneath the controller, but the controller is never
  destroyed.

* Item controllers are short-lived objects. These controllers are often used
  to decorate an array of items and are destroyed and created during the life of
  an application.

* You don't necessarily need to create your own controller, especially if you're
  not holding onto application state. The router `actions` object is a great
  place to save, create, and delete models.

A Day in the Life of a Controller
---------------------------------

In this example I'm going to show a list of items and let the user select one at
a time. This kind of UI is often seen in a mater-detail view where the URL keeps
track of the selected item, but let's say you're just selecting an item in the
list for now. This is a pretty common use case where controllers shine.

To get started we'll setup a route with some dummy data and render a list of names.

```coffeescript
App.IndexRoute = Ember.Route.extend
  model: ->
    [
      {firstName: 'Gut', lastName: 'Bomb'}
      {firstName: 'Rock', lastName: 'Tastic'}
      {firstName: 'The', lastName: 'Fish'}
    ]
```

```
{% raw %}
<ul>
  {{#each}}
    <li>
      {{firstName}} {{lastName}}
    </li>
  {{/each}}
</ul>
{% endraw %}
```

If you've used Ember much this probably isn't earth shattering stuff.

But now it gets interesting. When the user clicks on an item we want it to appear
selected.

```
{% raw %}
<ul>
  {{#each itemController="name"}}
    <li {{action "select"}} {{bind-attr class="isSelected"}}>
      {{firstName}} {{lastName}}
    </li>
  {{/each}}
</ul>
{% endraw %}
```

```coffeescript
App.NameController = Ember.ObjectController.extend
  isSelected: false

  actions:
    select: ->
      @set('isSelected', true)
```

First we're telling `each` to use the `nameController` to wrap each name. Then we
send the `select` action to the `nameController` when an item is clicked.  The
`select` action changes the `isSelected` property which in turn adds the
`is-selected` class to the `li` element.

Notice the declared `isSelected` property at the top of the controller. It's a
good idea to declare this property, otherwise `isSelected` would be set on the
model.

But oh no! If we click another item it also gets selected without unselecting
the other one. [Try it out](http://jsbin.com/ikusok/8).

We need a way to unselect names. Let's move the state about which item is selected
to the parent array controller.

```coffeescript
App.IndexController = Ember.ArrayController.extend
  selectedName: null

  actions:
    selectName: (name) ->
      @set('selectedName', name)

App.NameController = Ember.ObjectController.extend
  needs: ['index']

  isSelected: (->
    @get('controllers.index.selectedName') == @get('model')
  ).property('controllers.index.selectedName')
```

```
```

By using a computed property on the parent array controller we can codify the idea
that only one item is selected at once. We're taking advantage of the fact that
actions bubble through the controllers and moving the `select` action to the
`IndexController` so that the logic can live close to the state it manipulates.

The `needs` declaration opens a line of communication from the `NameController`
to the `IndexController`. One could instead use the `parentController` property
on the `NameController` and trade in the explicit `needs` setup for more terse
syntax.

Setting the `selectedName` on the `IndexController` is not strictly necessary,
but it's a good habit to get into to make sure that you aren't accidentally
proxying a property to another object. It also helps developers see the state a
controller is responsible for at a glance.

With these changes we continue to keep the application state out of our models
and still remember the selected item as users move around the application.
The `IndexController` is a singleton controller that remembers which names is
selected, while the `NameController` is a short-lived decorator that presents
the `isSelected` property to the template.

So [now check it out](http://jsbin.com/ucanam/3866). It works!

Want a more in-depth look at Ember? [Check out the comprehensive Introduction to Ember.js online class](https://teamgaslight.com/training/courses/14-early-access-new-introduction-to-ember-js) that I've been updating with Ember's creators.

<a href="https://teamgaslight.com/training/courses/13-working-with-state-in-ember">
 ![](https://gaslight-blog.s3.amazonaws.com/intermediate-ember-controller-concepts/screencastad__1_.png)
</a>
