---
post_author: Chris Nelson
categories:
- Development
tags: []
post_title: Living the Dream with JRuby
publish_date: 2013-06-18T15:57:00.000+00:00
feature_post_image: "/uploads/superchris.jpg"
post_images: []
layout: post
current_gaslighter: true
slug: living-the-dream-with-jruby
permalink: "/blog/:slug"

---
I got into Ruby through [JRuby](http://jruby.org/). I was doing Java for about 10 years before I
started doing Ruby. What got me interested, like so many others, was Rails. I
was blown away by Ruby and Rails, but the surrounding ecosystem (native
libraries that needed to be compiled on each platform) made me sad. My initial
reaction was to attempt bring the awesomeness I saw in Rails to Java by
attempting to build a Rails-esque framework in Java.

Though I had some success, it quickly became apparent that what really made
Rails such a joy to build with is the Ruby language itself. I discovered JRuby
at about this same time, and there was a huge momentum around the project. Sun
Microsystems, the inventor of Java, hired the two leaders of the project,
[Charles Nutter](http://blog.headius.com/) and [Tom Enebo](https://github.com/enebo) to work on it full time and it quickly progressed
to the point where it could run Rails. All of a sudden we had all the amazing
developer productivity of the Ruby language combined with the mature, platform
independent ecosystem and solid engineering of the JVM. I was convinced we
were about to enter a bright and shiny future for the legions of developers
laboring in the Java mines writing EJBHome interfaces and hand-editing their
struts-config.xml files. Turns out my timing on this was a wee bit off ;)

Fast forward about 8 years to 2013. We at Gaslight are starting to see a
pickup in Ruby adoption in the enterprise.  It's common knowledge that the
enterprise tends to be more risk-averse in it's technology adoption. What
is now old news in the startup community, is just coming onto the enterprise
scene. But at this point, Ruby and Rails are no longer new and scary: it's
a mature tech stack with lots of success stories to back it up.

However, in many corners, the word still doesn't seem to have gotten
out there about JRuby. It seems like there's almost a disbelief that it could
possibly be so easy and seamless to take a vanilla Rails application and
deploy it to an existing Java infrastructure. Well, I'm here to tell you, it
really, really works. By adding the warbler gem, you can take a Rails app and
run a single command to produce a war file that you can drop into any app
server. JRuby automatically leverages Java standards like JDBC to
talk to databases. Most all of the popular Ruby gems that relied on native code
have versions ported to Java, and the right java-specific version
automatically gets installed for you by using JRuby.

It's also worth pointing that using any Java API from JRuby is transparent and
seamless. A jar file can be required into JRuby using Ruby's normal require
statement JRuby does a ton of automatic "rubification" to make the Java
libraries that you bring in easier to work with. Java getters and setters become
normal Ruby attributes, and Ruby blocks can be used to implement Java interfaces,
just to name a couple. All this means that when you need to use Java APIs
directly, you are still using conventional Ruby code, no extensions to the
language are necessary. We've taken advantage of this on application here at
Gaslight to get some huge performance gains by bypassing the Rails stack at
certain hotspots. I'll detail this in an upcoming post.

We're fortunate to be working with IPC, the company owned by SUBWAY&reg;
franchises, on their first JRuby on Rails project. The reception has been
overwhelmingly positive by developers as well as customers. Developers have
by and large had the same reaction to Ruby and Rails that I had when I first saw it.
We've been able to leverage things like active_admin to get things in front of
our customer fast and then iterate. And we're able to deploy our Rails app
into the existing PCI compliant environment thanks to JRuby.

This adds up to a big win for organizations that are larger and
have different needs, than the prototypical startup that would previously been
the sweet spot for Rails. We want to get the word out to large organizations
that you absolutely can have your cake and eat it too. You can deliver applications
quickly, with less code, inside the infrastructure you already have. Go [JRuby](http://jruby.org)!
