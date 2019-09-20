---
layout: post
post_author: Katherine Tornwall
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: A Beginner’s Guide to the Ember Run Loop
publish_date: 2015-08-14 14:01:00 +0000
feature_post_image: "/uploads/ember-01 (2).png"
post_images: []
slug: a-beginners-guide-to-the-ember-run-loop

---
Have you ever wondered why you always seem to get this error message when writing unit tests?

```
Error: Assertion Failed: You have turned on testing mode, which disabled the runloop's autorun. You will need to wrap any code with asynchronous side-effects in a run
```

As someone who forgets the Ember.run call in my tests frequently, it's frustrating. And it's even more frustrating for [Ember.js beginners](https://teamgaslight.com/training/courses/14-introduction-to-ember-js) who might not understand what is going on. Why do I need to wrap everything in a run when testing? Why does everyone say that I do not really need to understand the Ember run loop?

Coming from a SproutCore background, I thought I understood the run loop. Many of the [concepts in Ember](https://teamgaslight.com/blog/the-evolution-of-ember-a-look-at-its-past-present-and-future) are taken from SproutCore, including the run loop, computed properties, and the router. But there are a lot of other subtle details that I had no idea about until doing more research on the subject. It seems intimidating at first, but it is actually pretty easy to understand.

## So, who needs to understand the run loop?

Most Ember developers will tell you that you do not need to understand the run loop when you are starting out with Ember. For the most part, this is true. It is perfectly fine to think of it as "the magic of Ember" and pick up the details on a need-to-know basis.

After mastering other parts of Ember, you will inevitably run into a situation where understanding the run loop will be extremely useful. This becomes evident when testing your code and wrapping other Javascript libraries for use in Ember. But most of all, it's just nice to understand the details in how a framework tackles interesting challenges.

## What is the run loop anyway?

When an Ember app is running, it needs to react to any user interaction. In response to user interaction with the browser, Ember.js will create a run loop to execute any JavaScript that needs to run in response. Each function, usually referred to as a job, will be placed within a particular priority queue on the run loop and be executed in priority order. There are currently six priority levels, from highest to lowest priority:

1. Sync
2. Actions
3. Router Transitions
4. Render
5. AfterRender
6. Destroy

The Run Loop will loop over all of the jobs on the queue by taking the first with the highest priority. Then the job is executed, which may put more jobs on the queue. It continues looping through the list of jobs until all are complete, and then control is returned to the browser.

## Types of queues

We briefly listed the types of queues by priority above, but we didn't discuss the types of jobs that get sorted into each queue. Learning how jobs are sorted into each queue gives us an insight into how this makes Ember more efficient by using the run loop.

### 1) Sync

The sync queue is where bindings throughout the application are handled. If you have not heard of bindings before, they are what glue your JavaScript objects to the DOM. For example, you might use a binding for an input field value:

```
{% raw %}{{input value=sampleProperty}}{% endraw %}
```

There are two reasons that these sorts of jobs are in the top priority queue. First, Ember is kicking off run loops based on user interaction, such as entering a value into an input field. We need this new value before we can react to any changes. Second, we need to keep our objects in sync while performing actions. If we didn't, there could be synchronization issues across services, routes, and components that would be difficult to track down.

### 2) Actions

Actions sound like it would be the place where your controller actions are run, but actually, it is the place where asynchronous actions are performed. This currently consists almost entirely of resolving promises. Your promise handlers will often place more tasks on the sync queue to update the UI.

Any functions that are wrapped within an ```Ember.run``` call that does not have an explicit ```queue``` parameter are also called within the actions queue.

While we are on the subject, what about our controller and route actions? Where do they get executed? I had a lot of trouble finding information on this. I tried to investigate this using debugger statements, and it appears that it is called immediately, before any of the run loop queues are activated. This is interesting because it is still within a run loop but not within any of the queues.

### 3) Router Transitions

Before Ember starts to re-render the page, it will try to process any route transitions. This priority comes third so that we don't render anything we are going to immediately transition away from and let any sync and action jobs settle before moving away from the page.

### 4) Render

After all updates have settled, Ember starts generating the updated DOM. Notice that this queue comes after all of the big changes have settled, and our application state won't really change after this point. This means that Ember will only need to generate the new DOM once during this run loop. Since this is the most expensive step in the process, it is a big part of why the run loop makes Ember so efficient.

### 5) AfterRender

This section is run once elements have been added to the DOM. They may not be on the screen at this time because control has not been given back to the browser to allow for a paint. You will probably use this hook the most often out of any of the queues, since it is often used to initialize JQuery components in Ember.

### 6) Destroy

At the end of the loop, Ember will clean up objects as needed. This comes last to ensure that we don't access objects that have been destroyed. Now, accessing destroyed objects is still possible if you are accessing Ember objects inside of an asynchronous callback that executes in a **future** run loop, so you should always check if an item has been destroyed in those situations.

## Autorun loops

You may be familiar with the name "Autorun" because of the error message at the beginning of this post. Autorun loops are a regular run loop, but they are created by Ember in case you try to run Ember code outside of a run loop. Because of assumptions that are made to be safe, autoruns are slightly less efficient than using a proper run loop.

The way I've come to understand autorun loops best is to think of them like training wheels on a bicycle. They are there to protect us from falling over when learning how to ride, but the end goal is to be able to ride a bike without them. It is not recommended to rely on them, but they will catch you if needed.

The one thing to note is that while testing, autoruns will not be created for you. This is because there are testing scenarios that may result in hard to debug errors when using the autorun loop. Because of this, it is considered to be a best practice to always have your code running inside a run loop and not to rely on autoruns.

## When might you need to use this?

You usually do not have to think about starting a run loop yourself, since Ember already does this in response to interactions. However, there are situations where you will need to create your own run loops to have Ember work correctly. Here are some examples:

* Websocket response handlers

#### Example from [ember-websockets](https://github.com/thoov/ember-websockets/blob/5d2dd3413c54a7c6c36b1d3c380a5dde84d55277/addon/helpers/websocket-proxy.js#L80)

```
this.socket['on' + eventName] = event => {
  Ember.run(() => {
    ...
    forEach.call(activeListeners, item => {
      item.callback.call(item.context, event);
    });
  });
};
```

* Wrapping jQuery component callbacks

#### Example from [ember-select-2](https://github.com/iStefo/ember-select-2/blob/9df55428350142de9bb863c625938d43cac86616/addon/components/select-2.js#L451)

```
selectionChanged: function(data) {
  ...
  Ember.run.schedule('actions', this, function() {
    this.sendAction('didSelect', value, this);
  });
},
```

* Other asynchronous callbacks outside of Ember-land

## More information

Hopefully, I have been able to clear up how the run loop works a little more with this guide. If you still want to learn more about the run loop, there are plenty of good references around. Here are some links to resources that I have found the most helpful while learning about this myself:

[Ember Run Loop Handbook:](https://github.com/eoinkelly/ember-runloop-handbook)
This is an extremely thorough guide to the Ember run loop. If you have any further questions, I would check this resource first. There is an excellent reference table on how the run loop is used with various Ember.run functions [here](https://github.com/eoinkelly/ember-runloop-handbook#how-do-i-use-the-runloop).

[The Ember Run Loop by Jason Madsen, January 2014 Salt Lake City Ember Meetup:](https://youtu.be/G4DdNMLubgQ)
This is a good talk about the run loop that helped clear a lot of things up for me. If you can find the time to sit down and listen to this talk, I would recommend it.

[Ember Guides:](http://guides.emberjs.com/v1.10.0/understanding-ember/run-loop/)
There is a good visual demonstration of how the run loop works in the guides under the heading "An Example of the Internals."

[Backburner.js and the Ember Run Loop:](http://talks.erikbryn.com/backburner.js-and-the-ember-run-loop/#/)
The Ember run loop is built on top of a library called Backburner.js. This is a slide deck on how Backburner and Ember work together, written by the person behind the Backburner.js library and Ember Core team member, Erik Bryn. If you are interested in learning about how the run loop could be applied to other frameworks, check out the [Github repository](https://github.com/ebryn/backburner.js#simple-backbone-example).
