---
layout: post
post_author: ''
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Learn Some Angular 2 at the Playground
publish_date: 2015-10-28 22:17:00 +0000
feature_post_image: ''
post_images: []
slug: learn-some-angular-2-at-the-playground

---
Angular 2 is getting closer. I’ve been super excited to see things taking shape over the last few months, but it’s not always easy to dig in and try stuff out. As expected with a rapidly evolving project that isn’t beta yet, let alone 1.0, documentation is often either lacking or quickly out of date. I’m confident this situation will keep improving as we get closer to a 1.0, but for the impatient, there’s a better way:

![Use the source](http://blog.codinghorror.com/content/images/uploads/2012/04/6a0120a85dcdae970b016765373659970b-800wi.jpg)
Credit to Jeff Atwood for this

It turns out that in the Angular 2 source code there’s a whole bunch of examples that demonstrate how to use key features. And it’s actually pretty easy to get Angular to build and run if you know where to look. I cloned the main Angular repo and followed the instructions in this dev guide [here](https://github.com/angular/angular/blob/master/DEVELOPER.md).

It's pretty important that you have right version of Node (1.4.2). I tried first with the older version I had installed and it didn't work out. It looks like the gulpfile.js uses some es6 syntax and that just won't work in earlier Node.js versions it seems. You can skip the part about Dart, unless you are into Dart. Then by all means have at it. If you just want to run the JavaScript and TypeScript examples, running

```
npm install
gulp build.js
```

should be enough to get you going. Last but not least, there's undocumented task I found by combing through `gulpfile.js`. If you run:

```
gulp serve.js.dev
```

that will launch a server on port 8000. It's gotta do a lot of work building some stuff, so give it some time. It took a good several minutes to get through everything and show the message that it was listening on port 8000. Once it starts, hit (http://localhost:8000/playground/src/). You'll see a list of the example apps in `modules/playground`. In particular I found the routing, order_management, and person_management apps interesting. There's some features, like route parameters, I hadn't seen any documentation anywhere else on. Better still, if you want to try something out, you can make code changes and it will watch and recompile. It's only recompiling what you change, so it's a good bit faster than the first time you launch it.

In future posts I might dig into some of the stuff I learned poking around, but for now, my recommendation is to dive on in yourself. The water's fine :)


