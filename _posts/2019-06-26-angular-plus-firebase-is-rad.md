---
post_author: James Smith
categories:
- Development
tags: []
post_title: Angular + Firebase is RAD
publish_date: 2013-09-18T13:50:00.000+00:00
feature_post_image: "/uploads/afire-logo.png"
post_images: []
layout: post
current_gaslighter: false
slug: angular-plus-firebase-is-rad
permalink: "/blog/:slug"

---
I believe that when developing an application feedback from the product owner is king. I try to optimize this feedback by delivering working features quickly and with as little ceremony as possible. Especially early on.

This is one reason I have been so excited about rich client javascript frameworks such as Angular and Ember. They allow me to deliver near complete features with out ever touching the server. We can go from wireframes to delivering html with rich behavior added in by a framework like Angular fast. Doing this gives me a working interface to interact with and that can drive feedback. The only thing left for me to do is wire up this interface to a server.

For the most part I have been happy with this approach to rapid prototyping. Rails makes it easy to build a JSON API for our rich client to consume. As I said earlier though I want as little ceremony as possible.

This is why when I heard about Firebase I immediately had to investigate it. Because thinking about my persistence layer this early seems premature. Even if I am just writing trivial Rails code to serve JSON, I am thinking about and naming objects that may or may not even need to exist. Depending on the direction the interface evolves. The little I can think about how and what I need to store at this point the better. It might even be possible that Firebase is sufficient longer term but, for now it allows me to delay those thoughts.

Firebase already has supported adapters for both Angular and Backbone. The one that I am interested in at the moment is AngularFire. Which bills itself as "The easiest way to wire up a backend for your Angular app." In this regard it's is spot on.

##Getting Started
Getting started is as simple as I can imagine. You just include the FireBase and AngularFire scripts into your existing Angular app.

```
<html>
  <head>
    <title>Let's Prop each other Up</title>
  </head>
  <body ng-app="propUp">
    <div class="container" ng-view=""></div>

    <script src="bower_components/angular/angular.min.js"></script>

    <script src="https://cdn.firebase.com/v0/firebase.js"></script>
    <script src="https://cdn.firebase.com/libs/angularfire/0.3.0/angularfire.min.js"></script>

    <script src="scripts/app.js"></script>
    <script src="scripts/controllers/main.js"></script>

</body>
</html>
```
##Building an App
Not only does Firebase allow me to defer focusing on my backend. But, it also gives me some fun realtime data syncing features to play with. The AngularFire page calls this 3 way data binding.

This means we can persist data bound to a $scope to Firebase. Then whenever changes are made remotely we also receive those changes automatically.

This worked out great for the app I wanted to build.
It allows teams to publicly recognize accomplishments and hard work. These "props" are then displayed to everyone and tallied in realtime.

To do this all needed  to do was include 'angularFire' into my angular controller as a dependency and add a method for adding new props. Which our persisted to Firebase and sync'd back to everyone who is binding to $scope.props

```
  app = angular.module("propUp", ["firebase"]);

  app.controller('MainCtrl', function($scope, angularFire) {
    var ref;
    ref = new Firebase("https://angularfireblogpost1.firebaseio.com/props");
    $scope.props = [];

    $scope.addProp = function() {
      $scope.props.push($scope.newProp);
      return $scope.newProp = "";
    };

    return angularFire(ref, $scope, "props");
   });
```
AngularFire calls this form of binding "implicit binding". There is another form of binding called Explicit binding. It as you can imagine gives us some more control over when our data is persisted to Firebase.

Now I just need to use $scope.props in my template

```
<div class="hero-unit" ng-controller="MainCtrl">
  <div class="props" ng-repeat="prop in props">
    <p>{{prop.person}} was given props by {{prop.submitter}} for:
    {{prop.reason}}
    </p>
  </div>
  <fieldset>
  <input type="text" ng-model="newProp.submitter"/>
  <input type="text" ng-model="newProp.person"/>
  <textarea ng-model="newProp.reason"/></textarea>
  <button type="submit" ng-click="addProp()">Give Props</button>
  </fieldset>
</div>
```
It's as simple as that. Now we have a very basic App creates and persists data to Firebase that is displayed to any connected clients in realtime. Pretty neat!

I am excited about where AngularFire and Firebase are going, the kind of workflows they enable and the problems I can solve with them.

If want get started with there is a seed projected alled angularFire-seed (https://github.com/firebase/angularFire-seed). It has good examples of how to stub AngularFire and Firebase dependencies in your unit tests. As well as how to use the Authentication service AngularFire/Firebase provides.
