---
layout: post
post_author: Chris Nelson
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 3 Solutions for Using Meteor and AngularJS Together
publish_date: 2014-12-30 15:00:00 +0000
feature_post_image: "/uploads/meteor-01.jpg"
post_images: []
slug: 3-solutions-for-using-meteor-and-angularjs-together

---

Building rich client web apps has gotten a ton better in the past 5 years. Frameworks like [AngularJS](http://angularjs.org) and [Ember](http://emberjs.com/) have provided solutions to most of the problems you are going to encounter building the client side of your app: binding, routing, communicating with the server, to name a few. Building an app with either one is an enjoyable and productive experience.

One of the challenges that remains is how to build the server portion of a rich client app. I don’t mean that it’s difficult in the sense of unsolved problems or lack of prior art. What I mean to suggest is that it feels repetitive and tedious. As a simple for instance, let’s imagine building a rich client cookbook application in Rails. I’m going to build a client side resource that represents my Recipe and can persist to a server. This is trivial in either AngularJS or Ember.

Next I’m going to build a server side model that represents my Recipe. This is going to involve creating database migrations and a Ruby model class. Finally, I’m going to need to implement a server side route, controller and serializer to be able to respond to client requests. Now multiply this for each model in my application, and I haven’t even mentioned authentication yet. I’m already sad, and if it’s not something someone is paying me to do, I’ve probably lost interest. :/

[Meteor](https://www.meteor.com/) is a framework designed to holistically solve the problem of rich client web app development. What intrigued me about Meteor most is the ability to express the domain model of my application exactly once and have server side persistence provided for free. Meteor persistence is real time, so all changes to models are broadcast via web sockets to all connected clients.

Meteor also has the most enjoyable developer experience I’ve ever seen for OAuth integration. By adding a single Meteor package, you have solved the problem of adding GitHub, Twitter, or Google authentication to your app. What’s more, Meteor’s solution is smart enough to virtually hold your hand as you go get the API keys you need and configure your app. All this put together make it incredibly easy and fun to get started building a Meteor app.

As I dug in just a tad more, however, I found the frontend framework that Meteor ships with to be less desirable. The holistic approach of Meteor has led some to accuse Meteor of “trying to boil the ocean.” Regardless of which side of this debate you come down on, being able to use one of the existing best of breed client side frameworks is compelling, and AngularJS is my current favorite. I did some research and found that a few people had already started investigating how to use AngularJS with Meteor. I’m going to discuss three different solutions out there for solving this problem.

The first two solutions are quite similar. In fact, similar enough that we may end up merging at some point. Both are Meteor packages derived from the same roots, but they have slightly different features. The third takes a different approach entirely, and it’s interesting that it could also be applicable for non-angular applications.

## [`superchris:angular-meteor`](https://github.com/superchris/angular-meteor)

This is a meteor package developed by yours truly. I came across an existing pre-1.0 meteor package that was no longer being maintained, added some features that I found useful and updated it to be compatible with Meteor 1.0. It’s available right now as `superchris:angular-meteor` using the standard meteor package system. The GitHub repo explains in detail how to use it and links to a couple example apps. Under the covers, it allows you to use Angular inside your Meteor app by solving a couple key challenges.

The first is bridging the reactivity concept of Meteor with the binding system of AngularJS. angular-meteor does this by providing a $collection service that wraps Meteor collections and exposes them as property on an AngularJS $scope. angular-meteor adds the necessary Meteor reactive plumbing such that your $scope is notified when anything in the collection changes. angular-meteor also gives you pagination and support one-to-many relationships on Meteor collections.

The next problem angular-meteor solves is how to avoid collisions between the Blaze template engine that Meteor ships with and AngularJS. Specifically, both want to use the {% raw %}{{}}{% endraw %} to indicate expressions. Unfortunately, only AngularJS is flexible enough to allow us to change that and so the approach taken is to configure AngularJS to use [[ instead of }}. That means any existing templates will need to change, but it should be possible to do this with a simple search and replace. 

## [`urigo:angular-meteor`](https://github.com/Urigo/angular-meteor)

During the course of development, I learned that another group of developers had been working on the same problem in the same way, even to the point of arriving at the same name. Many of the features of this package are the same, with support for relationships being the notable exception. I’ve been in touch already, and It’s likely that moving forward we’ll end up merging these packages. This package is available as `urigo:angular-meteor`.

## [Asteroid](https://github.com/mondora/asteroid)

Asteroid is a completely different approach to the problem. It’s a much more ambitious approach and in some ways more interesting. Rather than bundle Angular into Meteor and solve the challenges with that approach, Asteroid completely separates Meteor from the front end. Meteor implements a protocol called DDP to communicate between browser and server. The protocol is a simple, lightweight protocol of JSON payloads over web sockets, and it’s quite well documented. The entire spec only takes a few minutes to read and for me is one of the most interesting things about Meteor.

Because DDP is a simple, open protocol, Asteroid has taken the approach of re-implementing it in JavaScript without any Meteor dependencies. It ships as Bower component, and this means you can implement your frontend in whatever framework you want; not just angular. I’m really intrigued by this approach and also with the idea of alternate DDP implementations. Expect a future post to talk more about this.

Lastly but not lastly, I’ll be talking about this at [CodeMash](http://www.codemash.org/) next month. Stop by and say hi if you’re there! I'm also looking forward to teaching a [three-day AngularJS workshop in Miami March 16-18.](https://teamgaslight.com/training/courses/18-mastering-angularjs)
