---
layout: post
post_author: Chris Nelson
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Building AngularJS forms with JSON Schema
publish_date: 2014-05-13T19:00:00.000+00:00
feature_post_image: ''
post_images: []
slug: building-angularjs-forms-with-json-schema

---
## Formtastic and simple_form, how we miss you

Around 3 years ago I came to the realization that building web applications using client side
MVC frameworks was the future. I've embraced the approach whole heartedly, but not without occasional sadness. I and many of the developers I worked with have been quite spoiled by
Rails, in a good way, and giving up some of the view helper goodness has been known to cause developer unhappiness. In particular, form
building is not nearly as much in today's client side frameworks and it has been with excellent form building
gems like formtastic and simple_form.

There's been a few attempts build something to fill this need, but none have been quite what I wanted so I
decided to have a crack at it myself. What I came up with is an angular directive I've called [schema_form](https://github.com/gaslight/angular-schema-form).
It takes json schema and builds a form.

How does that help? What is json schema and why should you care? I'm glad you asked...

## Why you should care

It turns out to be a good bit easier to build something like simple_form on the server side using a framework
like Rails. The reason for this is that when you are tightly integrated with a server side framework it's
possible to ask the framework for all the information you might need to know about your model. You can ask for
the types of each property and use this information to build the right kind of input for each one. You can even reflect
on validations to display the appropriate styling.

But on the client side there's no easy way to get this information. It's fairly simple to build a client side form builder, but
if it's client side you need to either duplicate the information from the server side in client side code, or have something on
the server side that emits metadata in some format the client side can understand. And in order to avoid tightly coupling your
client side form builder to your server side framework, it would be awesome if there was some agreed upon format for expressing
this data. Hmm, perhaps if there was an easy way to express schema information as json...

## Enter [json schema](http://json-schema.org)

Someone wise once said that the best thing about standards is that there are so many choose from. Clearly having a standard
is no panacea, but in this case json schema gave me a way to express exactly what I needed without much fuss. I'm not going to try to break down all of json schema for you, the docs at json-schema.org do a pretty good
job of that already. But let's go through a simple
example:

```json
{
  title: "Thing"
  type: "object",
  properties: {
    title: {
      type: "string",
      title: "Title",
    },
    good: {
      type: "boolean",
      title: "Good?"
    },
    level: {
      title: "Level",
      type: "string",
      enum: ["low", "medium", "high"]
    }
  },
  required: ["title"]
}
```

This describes gives a human readable title, "Thing" and specifies that Thing must be an object. For the
purposes of form building, object is the only type that makes sense. We then see an object that
describes each property of Thing. Each value of the properties object is itself a json schema object. We see
that this object contains two string properties and one boolean property. Further, we see that one of the properties,
level, is contrained to an enumerated list of values.

It turns out this information is enough to build a [reasonable form for editing our Thing](http://gaslight.github.io/angular-schema-form/)

## Where do schemas come from?

Because, json schema is a spec with some amount of interest, there's a chance your server side framework of
choice may already support it. In the case of rails, there was nothing listed on json-schma.org but a little
google turned up a decent gem that generated json schema from activerecord. It didn't yet do anything for
validation but I've started a fork that does.

## The vision thing

There's a lot more that can done with json schema that just building forms. One of the other go to tools in
our toolbox as rails devs is active admin. Active admin is a great tool for deploying usable admin UIs with little
if any code. Nothing like it exists in the client space just yet. JSON schema could serve as the mechanism
for conveying the data needed for not only forms, but client side models and entire admin UIS to be generated.
This is an avenue I'm very much interested in exploring.

And of course admin UIs are fairly limited in usefulness without support for relationships between objects. This is
something JSON schema supports through the JSON HyperSchema spec. Like the rest of the spec, it's quite light weight
and easy to understand.
