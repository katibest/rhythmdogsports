---
layout: post
post_author: Chris Nelson
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: How to Create a Configurable AngularJS Directive
publish_date: 2013-10-09T19:30:00.000+00:00
feature_post_image: ''
post_images: []
slug: how-to-create-a-configurable-angularjs-directive

---
In our [CampNG class](http://gaslight.co/training/courses/2), we have a section on adding validations and error messages. Having to add a span for each possible validation error is pretty tedious, so I thought I'd have a go at writing a directive to do it. One thing that was interesting about building it was making it configurable. I haven't seen much info out there about how to do this, so I thought I'd talk about how I did it.

The validation directives in Angular gives us an `$error` object on each input with a key value pair for each validation. The key is the type of validation and the value is a boolean with true indicating a validation error. What we want is to take this object and produce an error message for each validation and display it if that validation is failing. We'd also like to be able to configure the error message for each validation. The way to do this is to have our directive depend on an `ErrorMessages` service which is in charge of grabbing the error message for a given validation. The directive code looks like this:

````javascript
angular.module("validationErrors").
  directive('validationErrors', function(ErrorMessages) {
    return {
      restrict: "A",
      scope: {
        errors: "=",
        errorClass: "@"
      },
      controller: function($scope) {
        $scope.errorFor = function(errKey) { return ErrorMessages.error(errKey);};
        $scope.buildErrorClass = function() { return this.errorClass || "inline-help text-error"; }
      },
      template: "<span ng-class='buildErrorClass()' ng-repeat='(errorKey, isError) in errors' ng-show='isError'>{% raw %}{{errorFor(errorKey)}}{% endraw %}</span>"
    };
  });
````

The next trick is to make our `ErrorMessages` service configurable. Turns out we need to use a provider to do this. Defining a provider is just a more verbose way to define a factory or service. In fact, both factory and service delegate to provider as described in [this excellent post](http://iffycan.blogspot.com/2013/05/angular-service-or-factory.html). But providers can do something services and factories can't: they can be injected into a config block. This allows us to define a property on our provider that holds the messages and then configure them in our application. Here's what our provider looks like:

````javascript
angular.module("validationErrors").provider("ErrorMessages", function() {

    this.errorMessages = {};

    this.$get = function() {
        var errorMessages = this.errorMessages;
        return {
            error: function(errorType) {
                return errorMessages[errorType] || errorType;
            }
        };
    };

    this.setErrorMessages = function(messages) {
        this.errorMessages = messages;
    };
});
````

Notice the `$get` method. This is where we define what our provider returns when `ErrorMessages` is injected. In our application's module config, we can have `ErrorMessagesProvider` injected and pass it the messages for each validation. This is what it ends up looking like:

````javascript
var app = angular.module('plunker', ["validationErrors"]).config(function(ErrorMessagesProvider) {
  ErrorMessagesProvider.setErrorMessages({required: "Ya gotta have it!"});
});

app.controller('MainCtrl', function($scope) {
  $scope.name = 'World';
});
````
We define our application module, listing the `validationErrors` module as a dependency. This allows us to have `ErrorMessagesProvider` passed to us in the config block and set our error messages. The GitHub repo for this directive is [here](https://github.com/gaslight/ng-validation-errors), and here's [the obligatory plunk](http://plnkr.co/edit/51dEq9) showing it's use. Last but not least, I feel obligated to point out that we love building rich client front end web apps, and if you need some help [let's talk](mailto:hello@gaslight.co).
