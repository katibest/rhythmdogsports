---
layout: post
post_author: James Smith
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'Interoperability in Elixir: Dealing with the world outside the Beam'
publish_date: 2015-10-16 15:18:00 +0000
feature_post_image: ''
post_images: []
slug: interoperability-in-elixir-dealing-with-the-world-outside-the-beam

---
<iframe width="640" height="360" src="https://www.youtube.com/embed/b-xwM4i62q4" frameborder="0" allowfullscreen></iframe>


Ports, Nifs, and Interfaces, Oh my! Elixir is an incredibly powerful language that sits on top of the battle tested and reliable Erlang ecosystem. This power is a big reason I am excited about building applications in Elixir. It enables us to write more of our application's stack in Elixir itself--especially compared to previous languages I have used. Still, not everything can be written in Elixir. Sometimes you have to interact with the outside world, other tools, the operating system, or other code bases written in a completely different language. 

Thankfully, Erlang, and by extension, Elixir received a ""plays well with others"" award in kindergarten! The Erlang ecosystem gives us several tools to work with other systems, processes, and code bases. In this talk, I'll cover the basics of each type of interoperability, as well as the pros and cons of each. These include: Nifs: powerful native extensions Ports: allow external programs to be treated like any other Erlang process Jinterface: gives us interoperability with the Java Virtual Machine These tools enable us to tap into the power of other ecosystems and make it easy to fit Elixir into our existing systems. This can be an excellent way to introduce Elixir into your organization and solve problems well-suited to Elixir.
