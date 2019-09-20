---
layout: post
post_author: Chris Nelson
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 4 Reasons We're Having Fun Programming Elixir
publish_date: 2015-09-23 12:50:00 +0000
feature_post_image: ''
post_images: []
slug: 4-reasons-were-having-fun-programming-elixir

---
Ruby on Rails has been our go to server side technology for quite a few years now, but we’re always evaluating other things coming down the pike. On the client side, there’s been a number of hugely productive newcomers, but on the server side nothing has created the same level of excitement and enthusiasm I felt when I came to Rails. Until quite recently, that is. 

I’ve been playing around with [programming Elixir](https://teamgaslight.com/blog/the-philae-experiment-landing-an-elixir-app-on-meteor) off and on for a few months, and I’m feeling that same buzz and excitement. What’s more, it’s just a blast in the same way that I felt when I first started playing with Ruby. I thought it would be worth sharing some of the reasons behind my giddiness.

## The Community Is Amazing
One of the things that drew me into Ruby on Rails is that some of the smartest people I know were into it before I was. The first time I saw Rails was when Jim Weirich showed me the famous [“How to Build a Blog Engine in 15 Minutes with Ruby on Rails” video](https://www.youtube.com/watch?v=Gzj723LkRJY) from DHH. The fact that Dave Thomas, author of some of the most widely read and respected books on Ruby and Rails, has jumped into Elixir with both feet has definitely made an impression on me. 

As a technologist, it’s a huge challenge to decide which new and shiny things are really worth the time to investigate more fully. One of the factors in this decision for me is people whose opinion I’ve seen validated over many years. Dave is certainly one such person. I remember seeing Dave tell us about Ruby and No Fluff Just Stuff, an open source Java conference, at least 10 years ago. I was initially quite skeptical, which is a good default position on new and shiny things, but several years later I made the switch to Ruby myself. 

It’s not just Dave Thomas; it’s also the amazing progress of the whole Elixir community. In the last year, the amount of progress has been really impressive. Building an app with Phoenix feels every bit as productive as building an app with Rails. The number of hex packages available is growing rapidly, and so far we haven’t had trouble finding what we’ve needed. It’s certainly a new community, but the energy is really palpable. 

## Not Object Oriented, Process Oriented
The familiar syntax of Elixir makes it easy for Rubyists to jump in, but if there weren’t other compelling reasons to dive into the Elixir pool, that wouldn’t be a reason do it. In fact, a lot of what seem like familiarities are only skin deep. Even something as simple as variable assignment `a = “5”` mean something subtly different in Elixir. Rather than scary and off putting, I’ve found this really made me want to dig in more and understand the differences. There’s probably a whole article on the language features I love about Elixir, but I’ll try to distill it down to this: I get to reap some of the key benefits of functional programming on top a runtime that has processes as core level of abstraction.

This second point was really exciting to me as I dug into Elixir. We’ve spent some time at Gaslight [learning Clojure](https://teamgaslight.com/blog/why-were-learning-clojure) this year, and one of the main reasons we did so was to better understand what advantages functional programming offered us over object oriented. I enjoyed the adventure and learned a lot, but in the end, I never quite fell in love with Clojure. Part of the reason is that I’ve designed systems using Objects for a such a long time that it feels quite natural how to organize an application this way. With functional programming, I felt a little lost and adrift somehow. This hasn’t happened to me with Elixir. Instead of just being a functional language, Elixir brings the core concept of processes. Processes are light weight, maintain per process state, and communicate purely by passing messages. In a lot of ways, they arguably embody the original vision that the inventors of object oriented had in mind.

The other great thing about Elixir (really Erlang) processes is that they finally bring a viable answer to many of the challenges of decomposing a large system into services (microservices architecture). Deployment, versioning, and managing processes are all provided out of the box by the Erlang OTP framework, which has been battle tested running a large percentage of the world’s telecommunications traffic for decades. Because OTP has well thought out solutions, it’s possible to break off pieces of a system into services and distribute them with much less ceremony and effort.

## The PEEP stack
The PEEP Stack, a term coined in this [blog post](https://medium.com/@j_mcnally/the-peep-stack-b7ba5afdd055), stands for Phoenix, Elixir, Ember, and PostgresSQL. Phoenix is shaping up as the go-to web framework in the Elixir community. It seems like it might well be the killer app for Elixir and bring more developers into Elixir. It’s very familiar if you're coming to it with Rails experience, and this is by design. Local(ish) Elixir hero Chris McCord developed the framework to be something he could easily slide into projects as a Rails replacement.

Phoenix feels just as productive as Rails to jump into, but there a few reasons it feels even better for building “ambitious web applications” to steal an Ember.js term. Building RESTful JSON APIs feels really natural, and even simpler than it does in Rails. And the built-in support for clients collaborating with one another in real time via web sockets is extremely compelling. This key feature is called channels in Phoenix and is the inspiration for ActionCable in the newest version of Rails

## It’s Insanely Fast
I’ve always resisted the performance argument for choosing a language. I well remember the argument being made against Java back in the 90s, and now Java is widely regarded as one of the highest performance platforms for large applications. So I took this argument with a grain of salt, but after I saw that the language and ecosystem felt every bit as fun and productive as Ruby, I started coming around. The fact that Erlang grew out of the telco space means it has been designed to support ROFL scale performance and uptime from the beginning. This isn’t nothing. We’ve frankly struggled to get the performance out of Ruby we would like and having to fight with this less would make us very happy. And lest you think “Does Telco experience translate to web apps?” it’s worth checking out this [ about the WhatsApp engineering team](http://www.wired.com/2015/09/whatsapp-serves-900-million-users-50-engineers/) from a few years back.
