---
layout: post
post_author: Patrick Hildebrandt
current_gaslighter: false
categories:
- Development
- Design
tags: []
permalink: "/blog/:slug"
post_title: 5 Ways to Write Sass That Plays Well With Others
publish_date: 2016-04-29 13:15:00 +0000
feature_post_image: "/uploads/Sass-That-Plays-Well-With-Others.png"
post_images: []
slug: 5-ways-to-write-sass-that-plays-well-with-others

---
Sass is an excellent tool for front-end designers and developers. It can improve the CSS authoring experience and help you quickly build out your design system. Like any tool, however, it can be misused. When working with teams or working on projects that will be handed off to a client, it is important to maintain your Sass in a clear, easy to understand way so that others can quickly learn how to implement and iterate on the design system.

Here’s a few ways to make sure your Sass plays well with others:

## Limit Mixin Usage
Mixins can do lots of things. They can reduce repetition, programmatically produce chunks of CSS, and speed up development. However, if you’ve ever felt really smart about that mixin you just wrote, it might make someone else feel really dumb.

Simplicity is the key for mixin usage. Generating too much CSS with mixins can cause problems. This abstracts away too much of what is actually happening in your Sass, and your team members will need to constantly refer back to the mixin in order to figure out what’s going on. 

Using tools like [Autoprefixer](https://github.com/postcss/autoprefixer) can help you limit mixin usage. Your team will no longer need to remember which properties or values need to use a mixin to apply vendor prefixes.

## Shallow Nesting
Nesting is great to a point. It reduces the amount of typing and groups related pieces of Sass together. But if you nest your selectors too deeply, your Sass will become a labyrinthian mess. Other people might find this difficult to work with because it can be hard to tell how many levels of nesting there are at a certain point.

Shallow nesting also decreases the amount of overly specific selectors. Don’t fall victim to one of the classic blunders, a specificity war in your stylesheet.

I tend to limit my nesting to one to two levels deep for standard selectors with one additional level for pseudo selectors. In a few rare cases, I’ll nest to three levels.

## Organize Your Partials
How good does it feel to keep your closet organized? The best right? It’s so easy to find that shirt you were looking for. The same can be said for keeping your Sass partials organized.

When your partials are organized, it’s easy for team members to find that variable, mixin, or module that they need to edit. It also helps team members know where to save new partials that they might add.

I try to organize Sass partials into folders based on their function:

* Settings: variables and other configuration partials
* Tools: extends, mixins, etc.
* Base: reset/normalize, base typography and form styles
* Layout: layout grids
* Components: modules, buttons, special typography and form styles

## Use Source Maps
Similar to keeping your partials organized, using [Sass Source Maps](http://thesassway.com/intermediate/using-source-maps-with-sass) will help your team find exactly where CSS properties are being set.

When using Chrome DevTools, it will help you and your team members find the exact partial and line number where that selector is located. This is faster and and easier to understand than looking at compiled CSS in Web Inspector.

Source maps are magic. It makes me sad when they’re not used on a project that I’m working on. I know that it will take me longer to figure out how the Sass is organized, and my overall development time will be increased.

## Order Your Properties
This isn’t related to Sass alone, but ordering CSS properties also helps maintain consistency. Some people like to order alphabetically and others group properties by function (positioning, sizing, typography, etc.). I prefer to order by function. Again, organization helps you and your team members quickly find the thing they’re looking for.

Ordering properties also prevents accidental property duplication. Knowing how things are ordered can prevent some frustrating things. For example:

```
.selector {
  margin: 10px;
  width: 100%
  border-bottom: 1px solid #ccc;
  font-size: 1.2rem;
  line-height: 1.5;
  padding: 10px;
  margin: 20px; /* duplicate margin property */
}
```
Whoops. Before I started ordering my properties, I made this mistake more than I’d like to admit. I’d repeatedly be editing the first margin property and nothing would change. I’d ask myself: “Why? Why isn’t this working?” Of course, the second margin property is overwriting the first. Imagine how annoying this could be for your team members.

When writing Sass, I try to keep things in the general order that I like, but I tend not to worry too much about it. [Running CSScomb](http://csscomb.com/) when I’m done will do all the final ordering for me. CSScomb also helps keep spacing, line returns, indentation, and other things consistent. Again, I love consistency and organization. It’s the best.

## Use a Methodology
This is not necessarily a Sass thing, but I think it’s still helpful. Using a CSS methodology like [BEM](https://css-tricks.com/bem-101/) or [SMACSS](https://smacss.com/) can help keep things organized for your team. It will also help maintain consistency for those times when multiple people on a team are writing styles. No more using dashes and underscores interchangeably. 

Whenever you’re in doubt about when to introduce complexity into your Sass, think about how long it will take to explain to your team members and how long it will take for them to understand it. If it would take an entire blog post to explain and justify, then maybe it’s not worth it. Remember the [KISS Principle](https://en.wikipedia.org/wiki/KISS_principle), and you’ll do great.


