---
layout: post
post_author: Mitch Lloyd
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'The Evolution of Ember: A Look at Its Past, Present and Future'
publish_date: 2014-10-28 14:00:00 +0000
feature_post_image: "/uploads/ember-01.png"
post_images: []
slug: the-evolution-of-ember-a-look-at-its-past-present-and-future

---

Let me take you back to January 2013 and the darkest days of Ember. For my co-workers and I, it was the heyday of “Ember vs. Angular.” We knew that each had a lot
to offer over our current Rails and Backbone stacks, but OMG, WHICH ONE TO CHOOSE?

### Ember 1.0
Ember spent over a year in 1.0 "prerelease" status, and the ethos of developers
that tried it seemed to be "it's too big and too bleeding edge."  Angular,
on the other hand, was stable enough to use on real projects and allowed us to
add [just the sprinkles][sprinkles] that we had grown accustomed to using in Rails
and Backbone.

### Ember Data
Even after Ember 1.0 finally arrived another concern remained: What would become
of Ember Data? People asked if it was [fundamentally broken][ember-data-broken]
while [several][epf] [alternatives][ember-model] to Ember Data began to arrive
on the scene. Development activity slowed to a crawl during the summer
of 2013.

### Documentation
Developing with Ember before 1.0 involved a lot of code reading. Not necessarily
all bad, but the core team routinely got the feedback, ["your documentation
sucks."][documentation-sucks] Ember already had a reputation for having a big learning curve and poor documentation compounded the problem.

Turning Around
------------

As much as I enjoyed Ember's ambition, I'll admit that I was bearish on
the future of the framework. Angular was the next obvious step beyond Backbone and I worried that Ember had bit off more than it could chew.

But somehow it slowly, painfully turned around. Even with a strong community of
contributors, it's hard not to imagine Yehuda Katz extracting Tomster from that
swamp in the Dagobah system using the force. Or maybe that's just me.

As our [Gaslight CEO so aptly told Yehuda][chris-moore-comment]:

> You go into really hard things, power through it, and everything gets
> better.

[Ember 1.0 finally arrived][ember-1] and Ember Data [started getting the
love it deserved][ember-data-transition]. [Trek Glowacki][trek] led the charge
to bring the documentation up to snuff.

The project adopted a six week release process inspired by Google Chrome and
introduced feature flags to allow new experimental features in the code base.
At the same time, the core team showed commitment to backwards compatibility.
Ember became stable and predictable. Confidence and excitement was building.

Ember's Strengths
------------

Today Ember may not be as popular as Angular or Backbone, but I am bullish about
its future. We've seen a [growing number of companies][ember-users] using
Ember, but to me, it's all about the benefits for developers.

### The Router
It's hard to explain how much this part of Ember just works and addresses many
pain points in front end development. Maybe the best thing we can say is that it
is a high priority [feature for Angular 2.0][angular-router] and that when
well-known Ember developers began getting more involved with React, it [was
quickly ported][react-router].

### Computed Properties
Composing state with computed properties in Ember is truly a joy. When it comes
to managing state over time, I agree with Alex Matchneer that Ember has ["clearly
tamed that beast"][taming-state]. The expressiveness achieved with [Ember's
computed property macros][computed-macros] is beautiful and [it's only getting
better][ember-cpm].

### Ember Data
While there are still some lingering issues that need to be worked around
(notably a [couple][is-dirty-comparison] [issues][is-dirty-relationships] with
`isDirty`), Ember Data has reached a tipping point where its value is massive
and the cost of its quirks are minimal. It's heavily customizable and introduced
me to concepts like [side loading][] that I now can't live without.

At this point, no other library tackles the problem of a front-end data with as
much success as Ember Data does. However, one might point to [Meteor][meteor] as
an example of a full-stack platform that greatly simplifies client-server data
concerns, albeit at the cost of dictating your server-side stack.

What's Next?
------------

### Testing
While Integration Testing<sup>[1][]</sup> with Ember is quite pleasant,
testing the individual parts of an Ember application is awkward. I attribute
this struggle to the transition from using global namespaces to using ES6
modules and the need to use Ember's run loop in unit tests.

While there are [attempts to improve the situation][ember-qunit], I believe
there is significant room for improvement.

### Components
There is undoubtedly [confusion about the role of ember
components][component-confusion]. Their functionality seems to encroach on other
parts of the framework like views, item controllers and `render`. At the same
time, they provide a way to eliminate some of the less understood concepts in the
framework with a more general-purpose abstraction.

I enjoy the approach explained in talks from [Trek][ui-patterns] and [Ryan
Florance][x-foo-in-you] advocating small, reusable components on the scale of
what HTML already provides. However, the influence of "everything is a
component" ideas from libraries like Polymer and React encourage people to use
components to design larger, domain-specific portions of their application.

In my opinion, we're dealing with a problem in vocabulary, and as Tom Dale
indicates, [vocabulary is important for Ember][tom-dale-quora].

> [Developers need] both sophisticated tools and the right vocabulary of
> concepts to help them communicate and collaborate.

With time I hope we'll find the sweet spot for components or at least some
way to describe the different types of components that we build in our
applications.

### Services
The [introduction of services][services] came and went without too much fanfare,
but I anticipate this concept will have a significant impact on Ember. In short,
services are a declarative way to open a line of communication between parts
of your Ember application.

Services are little more than a name, and don't provide any functionality that
wasn't already possible in Ember. However, they provide a way of unifying what
were often ad hoc ajax requests and strained uses of controllers. I'm excited to
see how the community uses services when they step out from behind their feature
flag.

Training
--------
I've been excited to collaborate with [Tilde](tilde.io) on an update to their
Introduction to Ember online training available from their website or ours. Updating
the course has given me a chance to see how Ember has evolved over the past year
and a half. If you're interested in seeing my work, please [consider purchasing
early access to the training at a reduced rate.](https://teamgaslight.com/training/courses/14-early-access-new-introduction-to-emberjs)

#### Footnotes

<ol>
  <li id="integration-testing-footnote">
    The Ember guides use this term to describe tests that must operate on a
    running application.
  </li>
</ol>


[sprinkles]: https://twitter.com/wycats/status/262623660332957696
[ember-data-broken]: http://nragaz.com/post/41076138457/is-ember-data-just-unfinished-or-fundamentally-broken
[epf]: http://epf.io/
[ember-model]: https://github.com/ebryn/ember-model
[documentation-sucks]: http://emberjs.com/blog/2013/01/18/this-week-in-ember-js-4.html
[ember-1]: http://emberjs.com/blog/2013/08/31/ember-1-0-released.html
[chris-moore-comment]: https://teamgaslight.com/blog/ember-dot-js-with-yehuda-katz-and-tom-dale
[ember-data-transition]: https://github.com/emberjs/data/blob/v1.0.0-beta/TRANSITION.md
[trek]: https://twitter.com/trek
[ember-users]: http://emberjs.com/ember-users/
[angular-router]: https://github.com/angular/router
[react-router]: https://github.com/rackt/react-router/
[taming-state]: https://docs.google.com/presentation/d/1afMLTCpRxhJpurQ97VBHCZkLbR1TEsRnd3yyxuSQ5YY/edit#slide=id.g380053cce_1549
[computed-macros]: http://emberjs.com/api/#method_computed_alias
[ember-cpm]: https://github.com/cibernox/ember-cpm
[is-dirty-comparison]: https://github.com/emberjs/data/issues/1540
[is-dirty-relationships]: https://github.com/emberjs/data/issues/1514
[meteor]: https://www.meteor.com/
[1]: #integration-testing-footnote
[ember-qunit]: https://github.com/rwjblue/ember-qunit
[component-confusion]: http://ember.zone/the-confusion-around-ember-views-and-components/
[ui-patterns]: https://www.youtube.com/watch?v=AxyzonjWYuA
[tom-dale-quora]: http://www.quora.com/Client-side-MVC/Is-Angular-js-or-Ember-js-the-better-choice-for-JavaScript-frameworks
[services]: http://discuss.emberjs.com/t/services-a-rumination-on-introducing-a-new-role-into-the-ember-programming-model/4947/71
[side loading]: http://jsonapi.org/format/#document-structure-compound-documents
[x-foo-in-you]: https://www.youtube.com/watch?v=nVTXJyyBxQc

