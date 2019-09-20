---
layout: post
post_author: Bailey Miller
current_gaslighter: false
categories:
- Design
tags: []
permalink: "/blog/:slug"
post_title: 'React and You: A Designer’s Point of View'
publish_date: 2017-06-06 19:12:00 +0000
feature_post_image: "/uploads/jsx.gif"
post_images: []
slug: react-and-you-a-designers-point-of-view

---
*Have you or a loved one been exposed to React? You may be entitled to psychological compensation.*

Like most things that begin in isolation, my first React experience withered away in the waves of frustration. This was just the beginning though before I had a support system in place to aid the learning process of this JavaScript library. It wasn’t until I paired with other designers and developers on a full-time project that I finally saw the beauty and usefulness of React not only for developers but also for designers.

Here at Gaslight, a designer's toolkit stretches beyond Adobe Creative Cloud and physical mediums. We also roll our own HTML, CSS, and sometimes a little JavaScript depending on the person. Our code usually lives within a framework like Ruby on Rails. However, on my latest project, I learned all sorts of new things when integrating my front-end workflow into Rails AND React.

React is one of the most loved JavaScript libraries out there right now. Here at Gaslight, it’s taken our office by storm, seeping into the majority of our current projects. The main premise of React is combining the power of JavaScript, HTML, and CSS into one concoction. While React is powerful and full of splendor, it can make a designer’s workflow hairy if not handled with care.

## What does a designer need to know about React?

I rode off into the night to try to get a better handle on React on my glistening steed (Google). I was faced with oodles of articles that did nothing to ease my mind. Buzz words like "virtual DOM" that may not even be buzz words, just words that made me crabby, settled like sand at the bottom of the ocean of my sad soul. I was excited to use React, but I realized that there *must* be other confused designers and front-end coders out there. There aren’t many resources out there for us. Documentation usually targets the ones writing the JS, not the HTML and CSS.

Once I did some more research, I found out that the problem was perhaps not a lack of resources for designers using React, but too many contrasting opinions. Information overload, at least for me, yields paralysis. If you find yourself in these shoes, here is a non-comprehensive list of the things you need to know as a designer in the React world:

* **Your HTML is now called JSX**

I know, the term JSX sounds uber sleek, like a million dollar piece of art in Snoop Dogg’s house. But from me to you, I’ll tell you straight up: JSX is pretty much just slang for HTML infused JavaScript combined into a single type of file called .jsx.

* **Use classNames instead of class**

Don’t question it! The big React man said we gotta do it! A simple guide to debugging: Step 1. Are you refreshing your staging instead of your localhost? Step 2: Did you type class instead of className again?

* **Components need an outermost div**

Don’t take it away! Your team will be sad! And you’ll probably get an error. This can make complex flexbox layouts tricky if you’re using them. But once you find ways to deal, it’s nice for organization’s sake.

* **Stuff will get weird**

This is the biggest given.

## The Problem

So once you’ve figured out how React allows you to combine everything into happy components, what’s the holdup? Well, it can be quite the predicament to figure out what to do with those gosh darn CSS stylesheets.

The React people seem to want CSS in the React component itself so that HTML, JS, and CSS can all be together and they can benefit from each other. Good design practices say something like Sass should be used to organize styles down to separate partials. Where does that leave a lone designer who just wants to make everyone and her Mama proud? How could I disappoint my designer friends and the designer within myself by using *inline* CSS of all things, after I’ve worked so hard to learn best CSS practices?

## Pick your poison

React can be scary. It seems to go against all the CSS best practices we’ve built up as front-end coders. I thought Sass solved all my problems, but I was blind to the ways of the world. It turns out some people [question the entire concept of CSS.](https://css-tricks.com/the-debate-around-do-we-even-need-css-anymore/) GEEZ.

So, there’s no more avoiding the problem even though I’ve tried. Where do we actually put those pesky styles? Here are the three options I’ve found:

- **Inline (CSS in JS)**

This method makes the React people happy. This can be done in a few ways, but the main principle here is that your styles exist **in your JSX file.** These CSS properties can be written as JS objects and imported into each component OR *all the way inline* using something that looks like this:

`<div style="margin: 20px; background-color: black;">`

**Pros:** You’re doing things the real React way, which unlocks the power of JS in your styles.

**Cons:** It feels strange, can be hard for other designers and developers to follow, and can make certain CSS features like media queries less straight-forward.


- **Business as usual CSS (Sass, Less, plain ol’ CSS files)**

If you’re using this method, your stylesheets will most likely look like they normally do in their comfy little folder. If you use a preprocessor like Sass, stylesheets might be split into partials. If you prefer vanilla CSS, that’s cool too.

**Pros:** You can use your pre-processor of choice (I like Sass) and you won’t have to change much about your styling workflow.

**Cons:** While your HTML and JS will live in harmony, your CSS will be on an island by itself and you won’t get to unlock all the power of JS.


- **CSS Modules (Somewhere in between inline and business as usual CSS)**

"Modular" can be a vague word to get a grip on. In this situation, it basically means that your CSS is split into parts corresponding to each component and used specifically just for that component. This method is the one that seems the most open-ended to me. The part that is open-ended is where these styles actually go (in their own CSS files, in a JS file, or what) and how specific they need to be to their corresponding component.

**Pros:** It seems to be the best of both worlds.

**Cons:** I can’t think of a whole lot.


## Sass + BEM + React = ?

Here’s what we did in my most recent project. We used Sass and BEM alongside a "traditional" file structure i.e. *no CSS within JS.* To me, it felt like a blend of CSS modules and business as usual method. While we did split our CSS into files that corresponded to the .jsx components, they were not scoped only to their respective components. Every Sass partial imported into the application.scss file, which was accessible by every component. Since we were using React AND Rails, the compiled CSS was imported into the main Rails view, as was the React. So basically, no CSS was written into JS directly.

![folder structure](https://s3.amazonaws.com/gaslight-blog/react-and-you-a-designers-point-of-view/folder-structure.png)

This is the breakdown of our file structure:

`base/` - Foundational CSS. These files flow into everything else. This includes fonts, variables, mixins, resets, transitions, typography, and utilities.

`components/` - Code corresponding to specific React components. This excludes chunks of code that already exist in the base/ or modules/ folders, which tend to be styles that are used throughout the app.

`modules/` - Sections of CSS that will be used in multiple places. This includes buttons, cards, forms, horizontalrule, links and select styles.

`pages/` - Page-specific styles. The only single page we have is a style guide.

`vendor/` - CSS from other people or frameworks.

I might try something more like the inline method in the future, but I was satisfied with my workflow doing the route that I did. I liked how each .jsx component had it’s own Sass partial with the same name, which helped keep things separate and straightforward.

## There are many right ways

My biggest takeaway has been this: there are many right ways to do something. Different codebases require different solutions. Don’t exhaust yourself probing the Internet looking for someone to tell you the one correct way, because it doesn’t exist. It’s okay to break the rules if your situation warrants it.

Take Internet advice with a grain of salt. Do you feel stupid? It’s not your fault. There are a lot of people writing articles who are bad teachers with big egos. It’s no wonder we’re all scurrying for help.  To combat this, I find that talking to real people about my learning journey helps. Online, there are opinionated people who might appear more persuading than the little guys, but your voice matters too. Wow, this just got deep. I guess that’s the nature of JavaScript, man.

## Further Reading

**For the philosopher:**

[The Debate Around "Do We Even Need CSS Anymore?"](https://css-tricks.com/the-debate-around-do-we-even-need-css-anymore/)

[Modular CSS with React](https://medium.com/@pioul/modular-css-with-react-61638ae9ea3e)

[React: CSS in JS](https://speakerdeck.com/vjeux/react-css-in-js)

[React Bits](https://github.com/vasanthk/react-bits)



**For the CSS in JSer:**

[Patterns for Style Composition in React](http://jxnblk.com/writing/posts/patterns-for-style-composition-in-react/)


**For the modular fellow:**

[Modularise CSS the React Way](https://medium.com/@jviereck/modularise-css-the-react-way-1e817b317b04)


**For the Sassy:**

[Styling React Components in Sass](http://hugogiraudel.com/2015/06/18/styling-react-components-in-sass/)


**For the CSS purist:**

[Why You Shouldn’t Style React Components with JavaScript](http://jamesknelson.com/why-you-shouldnt-style-with-javascript/)

[Against CSS in JS](http://keithjgrant.com/posts/2015/05/against-css-in-js/)

[React Inline Styles are Fundamentally Flawed](https://byjoeybaker.com/react-inline-styles)

**For the BEM fanatic:**

[Creating BEM Stylesheets for React Components](https://engineering.wework.com/creating-bem-stylesheets-for-react-components-c0d200309ed4)

**For the tool obsessed:**

[Complementary Tools](https://github.com/facebook/react/wiki/Complementary-Tools)
