---
post_author: Joel Turnbull
categories:
- Development
tags: []
post_title: I Like Pry But...
publish_date: 2013-08-19T13:24:00.000+00:00
feature_post_image: "/uploads/pry_logo.png"
post_images: []
layout: post
current_gaslighter: false
slug: i-like-pry-but-dot-dot-dot
permalink: "/blog/:slug"

---
Everytime I talk about Pry I invariably hear "I like Pry but…". More specifically, "I like Pry, but I wish I could step", and "I like Pry, but can't I do all this in debugger?". My article is an attempt to squash this, and get more people trying Pry. Let's start with the easy one.

## How do you step in Pry?

To step in pry you will need an extra gem. The available gems are `pry-debugger` and `pry-nav`. Both gems add the commands `step`, `next`, and `continue` to the pry REPL.

I had some bad times with `pry-debugger`. I recommend `pry-nav`, it's been so far, so good.

I would also recommend taking it straight from the [README](https://github.com/nixme/pry-nav). Add these shortcuts to your `.pryrc`:

    Pry.commands.alias_command 'c', 'continue'
    Pry.commands.alias_command 's', 'step'
    Pry.commands.alias_command 'n', 'next'

There are many gems like `pry-nav` that independently add enhancements to Pry's basic functionality. That's a good thing. You don't have to install all the things. And, you can use core Pry within JRuby and other environments that don't play well with C extensions.

Extensions aside, Pry core has some killer features of it's own out of the box.

## Pry Wins: The Short Sell

Here's the quick sell for Pry. Pry has a couple of apparent advantages over your standard Rails console or Ruby `debugger`. (When I refer to debugger, I'm talking about the fork of the `ruby-debug` library).

- Syntax highlighting
- Tab completion

I like using debuggers and REPLs and using them often. It's been stated that the average dev spends 60% of their time debugging. I spend most of my time debugging in a debugger, and I want colors, and for it to type things for me.

Hopefully, that's enough to pique your interest. But the value of Pry runs a lot deeper than that.

## Pry Wins: The Long Sell

Earlier, I blogged about why I liked Pry so much as a [discovery tool](http://gaslight.co/blog/pryme-time). I was thrilled to see so much excitement around [Conrad Irwin](https://twitter.com/conradirwin)'s [talk at RailsConf 2013](http://www.youtube.com/watch?v=jDXsEzOHb2M), and around Pry at the conference. I'd wager Pry shirts outsold any other at the swag table.

But why do people care? The things you can do in Pry, you can also do in debugger.

Pry isn't just an alternative to debugger. It's a REPL, more correctly an alternative to IRB. In fact you can use the `pry-rails` gem to replace IRB when invoking `rails console`. Having said that, I'd like to state the opposite. Using extensions, anything you can do in debugger, you can also do in Pry.

Two tools that do similar things differentiate themselves in small but important ways. It comes down to the user experience. Which tool is easier to learn and reason about? Which tool is more natural and enjoyable? Which tool is optimized to do things you are really trying to do?

My initial idea for this article was to compare the way you do things in Pry vs. debugger, and show how much better Pry is. Figuring out how to execute things like Pry's `cd` command in debugger took too long for me to continue to entertain this concept. Pry's `cd` command changes the value of self in the REPL. Maybe it is possible to do it in debugger, maybe it's not. The point is, the developers of Pry identified that that ability to is core to the kind of workflow that enables discovery. It's a core concept of working with Pry, and one of the first commands you learn and learn to love.

This ambivalence between Pry and debugger (and IRB) stems from the fact that many Rubyists don't consider a debugger or REPL an essential part of their workflow. Working on other languages and environments, I've learned how useful and essential a **good** one can be. Fixing bugs is only one piece of it. The other piece is discovering and understanding how our systems and libraries work, by being "inside" of them. Ruby is actually behind the curve here when you compare it with tools for other languages like the Chrome Developer Console, or the Smalltalk Debugger. I think Pry is changing that, and I think you should try it.

## More Cool Things About Pry

Interested? Watch Conrad's [talk](http://www.youtube.com/watch?v=jDXsEzOHb2M). It provides a simple example of using Pry for this discovery stuff I've been going on about. It also showcases some of Pry's coolest features:

 - Live code editing from inside the Pry REPL.
 - `wtf?`
 - `?` and `$` for code browsing
 - `play`ing lines of code
 - `cd` and `ls` for discovery
 - Running shell commands (e.g. `git diff`) from the Pry REPL
 - Better Errors with Pry
 - Launching Pry automatically on exceptions
