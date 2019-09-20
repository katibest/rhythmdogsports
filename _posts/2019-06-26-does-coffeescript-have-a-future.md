---
post_author: Joel Turnbull
categories:
- Development
tags: []
post_title: Does CoffeeScript Have a Future?
publish_date: 2013-09-04T14:32:00.000+00:00
feature_post_image: "/uploads/coffeescript.jpg"
post_images: []
layout: post
current_gaslighter: false
slug: does-coffeescript-have-a-future
permalink: "/blog/:slug"

---
CoffeeScript has made the landscape of JavaScript development so much more interesting. There is no doubt that CoffeeScript has emerged.  Rails and any other framework worth its salt ships with support for it. Github's internal style guide [promotes it](https://github.com/styleguide/javascript). We write it religiously at Gaslight.

But Kevin Rockwood recently came back from an Ember.js event in Chicago, saying some really interesting things about JavaScript. He pointed me to this [discourse thread](http://meta.discourse.org/t/is-it-better-for-discourse-to-use-javascript-or-coffeescript/3153/9), in which something significant happens. The core team of Discourse, a substantial open source project, make an official decision to stop writing CoffeeScript in lieu of JavaScript.

Knowing what we know about the landscape of JavaScript development and how it is evolving, I have to wonder if is this a signpost. Does CoffeeScript have a future?

## About the Author

It made Chris Moore visibly, physically ill when I said I came to Jay-Z through Kanye West. I'm a little trepidatious to admit I came into real JavaScript development through CoffeeScript.

My trepidation speaks to the palpable schism between writers of JavaScript and writers of CoffeeScript. I'm a CoffeeScript proponent, but I hope I can do this subject a little objective justice after reading some of the reverse takes on it:

- "Even if you've never programmed before the JavaScript example would make sense. The CoffeeScript is, well, illegible"
- "CoffeScript reminds me of VB…and nobody likes VB"
- "CoffeScript is not a language"

It's hard for me to look at function(). I like significant whitespace. These are stylistic arguments that are really uninteresting. There are more than a handful of valid language features that CoffeeScript provides that make choosing it a no-brainer for me.

There are similarly a thousand inconsequential, stylistic arguments that can be made against writing CoffeeScript. But there are handful of really valid ones. Interestingly, those arguments seem to become more relevant as time elapses.

## JavaScript is the Lingua Franca

Discourse's main reason for the switch was to encourage more contributions to its substantial open-source product. Some questioned whether this was valid. Does CoffeeScript really discourage some people from contributing? I can say from experience, yes.

We have taken heat multiple times for posting code samples on this blog in CoffeeScript. The response was similar each time we did it. A JavaScript developer would invariably comment: "I'm not even going to read this because I don't know CoffeeScript".

We took this discouragement as sour grapes. We posted CoffeeScript for two reasons. We wanted to support and disseminate something we really loved using, and CoffeeScript is really, really easy to learn.

I'm not so confident now. What has shifted? Tom Dale states in the thread "Every CoffeeScript developer knows JavaScript. The inverse is not true." This rings true for me, and it's not so much that something has shifted. It's just that this continues to be the case, and I don't foresee a future where it is not. Posting CoffeeScript in technical articles strikes me as presumptuous now.

## JavaScript Ain't Going Away

I hear smart people who understand both JS and CS say: "If you write CoffeeScript, you should take the time to understand what is really going on in the JavaScript it creates."

Apparently, because CoffeeScript is not really a language, we're all always going to have to care about and know about JavaScript. I don't see CoffeeScript reaching some critical mass as to prevent us from ever messing with it, I don't see source maps and compilers completely shielding us from it.

But CoffeeScript has emerged, and it has a major foothold. Will the division between developers just continue, will CoffeeScript adoption increase, or will JavaScript just win out in the end?

## JavaScript is Changing

The most compelling anti-CoffeeScript argument for me is complexity in integration. CoffeeScript compilation is always going to require extra machinery. As seamless as this has become, it's still a liability. Right now, it's obvious to me that the benefits of writing CoffeeScript outweigh the costs of integrating it. But is that always going to be the case?

JavaScript is changing. The next version of the language [ES6](http://net.tutsplus.com/tutorials/javascript-ajax/eight-cool-features-coming-in-es6/) is set to release with an impressive new set of features.

- Destructuring Assignment
- String Interpolation
- Classes
- Loops and Array Comprehensions
- Default Arguments and Splats
- Arrow Function Definitions
- and More!

These are examples of the features that make using CoffeeScript a no-brainer to me. It's obvious that the new JavaScript is heavily influenced by the improvements that were introduced CoffeeScript. Ironically, CoffeeScript has informed JavaScript so well that it may have rendered itself obsolete. How much do I care about clean syntax if JavaScript has all of the language features that CoffeeScript provides?

Regardless, I consider this coming evolution a win for both CoffeeScript and JavaScript.

## JavaScript Doesn't Have to Care

The changes to JavaScript are not only going to improve the language, but are also going to introduce a whole new set of incompatibilities that could potentially break every CoffeeScript compiled application in the wild today. At least, any CoffeScript application that doesn't explicitly target an older version of JavaScript.

In the thread Yahuda Katz states that the differences in ES6 are large enough that they could warrant a completely new redesign of CoffeeScript. It's not a patch or upgrade level solution. All of a sudden writing CoffeeScript could become a very real liability.

Regardless of the severity of the incompatibility, CoffeeScript is forever going to need constant love and support to keep pace with ongoing changes in JavaScript. Given that the JavaScript language is assimilating most of the features that CoffeeScript provides, how many people are going to be willing to continue that work?

## We <3 CoffeeScript

CoffeeScript has been a huge win. It's made JavaScript development magnitudes more enjoyable and accessible, it's gained support in all the major arenas, it's informed and transformed the JavaScript language itself, and it has proven itself to be an invaluable tool to thousands of developers.

I don't know what the future of CoffeeScript is. I do know that even if CoffeeScript died tomorrow, it will have done it's job and fulfilled its destiny. That makes me happy.
