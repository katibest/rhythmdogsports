---
layout: post
post_author: Chris Nelson
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: How to Drive a Serverless SOA Mashup Directly in the Browser
publish_date: 2013-11-08T15:00:00.000+00:00
feature_post_image: ''
post_images: []
slug: how-to-drive-a-serverless-soa-mashup-directly-in-the-browser

---
It's no secret that we've been investing heavily in client side development at Gaslight. We've been posting a lot on [Angular](http://gaslight.co/blog/?tagged=angularjs) and [Ember](http://gaslight.co/blog?tagged=ember.js) lately, and on [Backbone](http://gaslight.co/blog/?tagged=backbone.js) a ton before that. Recently I've been trying to understand just how far we can push the idea of client application development. I wanted to find out how practical it is to build applications with no server side code at all.

The other idea I've been thinking about a lot is SOA. Service Oriented Architecture, or SOA is an idea that has been big in the enterprise space for a few years now. The basic premise of SOA is about applications being able to communicate with one another in a standard way. However, the implementation has often left a lot to be desired. I've been on projects where we tried to do SOA from the beginning, and started by building a back end service layer first. Inevitably, what we found with this approach was that our services weren't exactly what we needed when we built the front end. We ended up needing to build two application simultaneously, with any changes causing work in both apps to be needed. Things felt heavy and slow.

On the other hand, when I've started building apps front end first, things came together much more naturally. With web apps built using frameworks like Angular and Ember you are building what is essentially a client-server application. The client code communicates with the backend by making HTTP requests containing JSON. This means rather than speculatively building an API service layer, we start with a client and build exactly what we need. This often ends up as a great starting point to build an API to expose to other applications.

The challenge
----------------

This got my thinking about whether I could build an SOA type app entirely client side. I decided to set myself this challenge: could I produce a "useful" app that used multiple Sass APIs directly from the browser. I hit upon this idea: build an app to implement random lunching at Gaslight. At our most recent retreat, we talked about the idea of more one-on-one lunches between Gaslighters as a way for us to build relationships with each other. This seemed like a doable task. We already had an app that grabbed a list of people in our Github org. And after a bit of experimentation, I was able get OAuth from the browser working with the google api. 

Thus, [LetsGitLunch](http://gaslight.github.io/letsgitlunch) (git it?) was born. LetsGitLunch is an app that picks a random member of your  github org and creates an event on your Google Calendar inviting them to go to lunch. The [code](https://github.com/gaslight/letsgitlunch) is, fittingly, on github. And to prove there's nothing up our sleeves, we host the app on github pages (hence no server code). 

How it works
---------------

The two key technologies that make LetsGitLunch possible are CORS and OAuth. CORS is simple, but an incredibly powerful tool for web developers. CORS stands for Cross Origin Resource Sharing, and it means that the browser is no longer limited to the Same Origin Policy, which previously limited javascript to make HTTP requests only to the domain from which the app was served. There are workarounds like JSONP but they're awkward and clunky; with CORS we are now able to build websites that communicate with 3rd party apis directly from javascript. There's lot more detail about CORS out there, and current versions of all major browsers support it.

The other key piece is OAuth. OAuth is a standard that allows a user to grant access to their data in one application to another application. In LetsGitLunch, this is what allows a user of Github and Google Calendar to say "It's cool for LetsGitLunch to be able to see who's in my Github organization and make calendar entries". Importantly, it does this *without* needing the the 3rd party application (LetsGitLunch in this case) to store or even have access to usernames and passwords. It's a little awkward as user experience for the first time we use the app. The good news is that the OAuth permissions we grant to LetsGitLunch are remembered by booth Google and Github so this is a one time thing.

What have we learned?
-----------------

I know it's a cliche to say "This is the future of web app development" so I'll just start by saying that this is the future of web app development. I'm saying this thinking more about internal business apps than consumer facing apps. Enterprises have been laboring to build SOAs for years, and for the reasons mentioned earlier it's often times been slow going. By building rich client web apps, we get a couple big benefits in a SOA environment. If we're building a new app, by building the front end first, we can have an exact specification for our services before we build them with a very high degree of confidence that they'll be what we need. For applications tie together existing services, the server side can be very thin, or even nonexistent in some cases. This leads to radically faster application development. We can build front end apps much faster with modern frameworks than we ever code before, and it just keeps getting better.

If you're doing SOA in your organization and you'd like to find out more about all this or just compare notes, we'd love to [talk to you](http://gaslight.co/contact). We're excited about where all this is going. It's an [amazing time to be a web developer](http://wekeroad.com/2013/08/22/js-frameworks-are-amazing-and-no-one-is-happy), and we couldn't be happier.