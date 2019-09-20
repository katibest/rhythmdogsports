---
layout: post
post_author: Michael Guterl
current_gaslighter: false
categories:
- Development
- Design
tags: []
permalink: "/blog/:slug"
post_title: Structuring CSS on the Filesystem
publish_date: 2014-03-27T13:25:00.000+00:00
feature_post_image: ''
post_images: []
slug: structuring-css-on-the-filesystem

---
## Introduction

Chris Moore already [pointed out][gaslight-bem] that CSS is the last frontier
of web development and that we're mostly using [BEM] to structure our CSS at
Gaslight. Six months has passed and we're still using [BEM] so that probably
says something.

We've now used [BEM] on multiple projects and we recently had a discussion
about organizing our stylesheets. We have been relying on [The Silver Searcher]
in order to find CSS blocks due to our lack of convention. When compared to
other assets (models, controllers, etc.) in a project this really didn't make
sense.

## Folder Structure

- application.sass
- bootstrap_overrides.sass
- components
    - header.sass
    - post.sass
    - search.sass
- elements.sass
- fonts.sass
- reset.sass
- utilities.sass
- variables.sass

## application.sass

In a Rails application this is your manifest file and it is typically included
in your application layout. No styles should exist in this file, it should just
be @import statements.

## bootstrap_overrides.sass

We've been using [Bootstrap] recently and this is required if you want to customize.

## components

This folder is possibly the most important part. Any component (block) should
have its own file in this directory. This makes finding where styles are
defined very easy.

Component files are also responsible for media queries that may effect their
presentation.

## elements.sass

Despite using BEM we still have styles that will apply to all elements and this
is the place for them.

These styles represent the baseline or starting point of your application.
They're a level above the reset and the rules should not override other rules
or contain nesting for elements that are not inherently nested (ul > li, 
thead > tr, etc).

```sass
h1
  font-size: 4em
  
hr
  padding: 2px 0

tfoot > tr
  font-weight: bold
```

## fonts.sass

Want to know what fonts are available in your project? This is where you look.

```sass
@font-face
  font-family: "Lato"
  src: asset-url('Lato-Regular.ttf', 'font') format('truetype')
  font-weight: normal
```

## helpers.sass

These styles are not specific to your application domain and they have a very
limited ruleset.

Below are a few examples of good helper classes.

```sass
.text--left
  text-align: left !important
  
.flush
  margin: 0 !important
```

Below is an example of something that should not be a helper class.

```sass
.featured
  color: green
  font-weight: bold
  font-size: 200%
```

We tend to use a lot of the helper classes from
[inuit.css.](https://github.com/csswizardry/inuit.css/blob/master/generic/_helper.scss)

## variables.sass

This is where any of your global variables should be defined. Want to define
specific colors to reference in other stylesheets, do it here.

```sass
$light-green: #1ECA6B
$dark-green: #1AAF5D

$font-base: 1.25rem
$line-height: 1.5rem
```

## Rails Specifics

application.css should only be a manifest and should not include any styles of
its own. You should only include manifest files in your views.

[gaslight-bem]: http://gaslight.co/blog/block-element-modifier "Block Element Modifier"
[the silver searcher]: https://github.com/ggreer/the_silver_searcher "The Silver Searcher"
[bem]: http://bem.info/ "bem.info"