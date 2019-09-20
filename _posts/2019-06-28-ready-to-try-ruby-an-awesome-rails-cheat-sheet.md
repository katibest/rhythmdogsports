---
layout: post
post_author: Katherine Tornwall
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Ready to Try Ruby? An Awesome Rails Cheat Sheet
publish_date: 2016-03-03 14:30:00 +0000
feature_post_image: ''
post_images: []
slug: ready-to-try-ruby-an-awesome-rails-cheat-sheet

---
Last week I wrote about
[making a Ruby and Rails course,](https://teamgaslight.com/blog/adventures-in-teaching-a-ruby-on-rails-programming-class)
and some of the lessons I learned in the process. One of the biggest lessons that I learned was that
Ruby and Rails is way too much for a single session. Part of that pain came from the Rails cheat sheet
created for the course because it was missing a lot of key information about Rails. We liked the
idea of limiting the sheet to a single page, but lacking focus on a single topic resulted in a few
frequently asked questions during the class that indicated that the cheat sheet wasn't quite right.

To start the process of improving my course materials, I have updated the Rails cheat sheet based on
feedback and experiences from the class. 
This version is entirely dedicated to information about Rails, which allowed for a few additions
that I think are pretty valuable for any developer getting started with Rails.

## MVC in Rails

The core concept in any framework is the architectural pattern it uses. This section is to help
remind people what each part of Rails' MVC pattern is used for and remind them of
the naming conventions used. I found that I often confused which objects were singular or
plural when I started with Rails and wish I had this kind of reminder when I was just getting
started with Rails.

![MVC in Rails](https://gaslight-blog.s3.amazonaws.com/creating-a-rails-cheat-sheet/mvc.png)

## Application structure

One of the more complicated things that you need to understand when building a Rails app is the
application structure, or where all the different files go. There are tons of files and folders in
a newly generated Rails app, and I definitely found it intimidating as a beginner. I have seen
cheat sheets that show the file structure before, but I tried to make what I needed when I was
starting out in Rails by adding comments next to folders and files to help explain what the
important files are.

![Application structure](https://gaslight-blog.s3.amazonaws.com/creating-a-rails-cheat-sheet/application-structure.png)

## Common controller patterns

The last big change to the cheat sheet was adding common patterns that you find in almost every
Rails controller. The method names seem to be intuitive now that I am an experienced Rails
developer, but when I was new it could get a little confusing if I had to write the code myself.
To help others with this problem, I came up with a short list of query methods that I see most
frequently in simple applications.

![Controller patterns](https://gaslight-blog.s3.amazonaws.com/creating-a-rails-cheat-sheet/controller-patterns.png)

## More about routes

When I was starting out with Rails, I think the hardest thing for me to remember was how basic
routing worked. I understood what CRUD actions were, but remembering the exact name for things is
something I always struggle with. To help with this, I've shortened down the
[table in the Rails guide that explains this concept](http://guides.rubyonrails.org/routing.html#crud-verbs-and-actions)
into something that fits on the cheat sheet. I had this in the original, but was able to add the
HTTP verbs by condensing the wording.

![Routes](https://gaslight-blog.s3.amazonaws.com/creating-a-rails-cheat-sheet/routes.png)

## Command line reference

I've started to find that there are a lot of different commands that you need to know to be able to
function in a Rails development environment. I got a lot of tips and tricks from my coworkers when I
started that made my day-to-day tasks significantly easier. I included the basics and these tips in
this section to pass on this knowledge to others.

![Command Line](https://gaslight-blog.s3.amazonaws.com/creating-a-rails-cheat-sheet/command-line.png)

I've really enjoyed trying to make a cheat sheet for something I use every day. Hopefully others
are able to find this work useful. If you have any comments or suggestions, let me know! I hope to
make more cheat sheets soon. 

Rather have the cheat sheet in a different format? You can find it as a single web page [here](https://rawgit.com/ktornwall/cheat-sheets/master/rails-4.html), or
download a [PDF version here](https://github.com/ktornwall/cheat-sheets/raw/master/rails-4.pdf).

