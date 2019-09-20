---
layout: post
post_author: Geoff Lane
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Why We're Learning Clojure
publish_date: 2014-10-07 14:00:00 +0000
feature_post_image: "/uploads/picking-clojure-v1.gif"
post_images: []
slug: why-were-learning-clojure

---
<br>At Gaslight we're most widely known for our work with Ruby and Rails. We're also highly experienced in everything JavaScript from JQuery to Ember to AngularJS and more. But sometimes we need to break out of what's comfortable in order to learn and grow. How do we keep up with future trends and opportunities? 

Never stop learning. Over the past few weeks, our development team made the decision to master Clojure as a group. We're trying to build a critical mass around this technology, so when we take on Clojure for a client project, we'll be confident in our depth of skills and know this language has longevity.

So why did we choose Clojure?

## It Fills in Gaps Where Ruby and Rails Don't Shine
> I know we won't be programming Ruby forever. Another language and environment will come along that will replace it. I just hope it's actually better than what we have now.
> [-- Doug Alcorn](https://teamgaslight.com/people/doug-alcorn)

We still love Ruby. We still think Rails is usually the best solution for building web applications. But like any tool it has strengths and weaknesses:

  1. The Ruby concurrency story isn't strong. Yes, you can use JRuby and Celluloid, but mutability by default doesn't lend itself to concurrency. And many of the libraries you might want to use are not thread safe.
  1. Ruby and Rails are almost always fast enough. Almost always.
  1. Object Relational Mapping (ORM) isn't always the right answer.
  1. Object Oriented languages aren't always the best solution.


## Functional Programming Is a Different Way of Thinking
> Functional programming is a programming paradigm, a style of building the structure and elements of computer programs, that treats computation as the evaluation of mathematical functions and avoids changing state and mutable data. [--Wikipedia](http://en.wikipedia.org/wiki/Functional_programming)

Functional programming languages are a different paradigm from the way we normally program in Ruby and JavaScript. Learning that paradigm allows us to learn new and different techniques for solving problems. Techniques that might not have been possible to learn if we stuck with our comfortable imperative and Object Oriented languages.


## Clojure Runs on the Server
> Clojure meets its goals by: embracing an industry-standard, open platform - the JVM; modernizing a venerable language - Lisp; fostering functional programming with immutable persistent data structures; and providing built-in concurrency support via software transactional memory and asynchronous agents. The result is robust, practical, and fast.
> -- [Clojure.org](http://clojure.org/rationale)

Clojure runs on the Java Virtual Machine (JVM) and takes advantage of all that it offers: high-quality garbage collection, high-performance native threads and extensive portability. Clojure adds onto all that with state-of-the art immutable and persistent data structures that are efficient. Clojure brings an interesting concurrency model with Core.Async. Plus, there has been a lot of development of other interesting libraries for logic programming, pattern matching, optional typing and many other things we'd like to try out and understand.


## Clojure Runs in the Browser
> A development platform with extensive reach, portability, multi-vendor support, an optimization arms race, sophisticated tools, implemented on all new devices, and a call for richer and more sophisticated applications - what more could developers want? A different language, that's what.
> -- [ClojureScript Wiki](https://github.com/clojure/clojurescript/wiki/Rationale#opportunity)

ClojureScript compiles into JavaScript and runs in the browser. It offers us a completely new way to think about programming client-side applications. JavaScript is not known as the most widely loved language in the developer community. But Clojure is a full-fledged Lisp that's well designed, concise and has good asynchronous support. It also interoperates with other JavaScript libraries. All this offers us a chance to use the same language on the client and the server without making sacrifices in either place.

Managing the inherent statefullness of a client-side app is tricky, and while JavaScript frameworks like React and [Ember](https://teamgaslight.com/training/courses/4) are beginning to address this issue, we are beginning to wonder if a more fundamental change in approach is worth perusing. Would a better language paired with one of these emerging frameworks provide us with a path that would allow us to better manage complexity? Clojure allows us to pursue answers to that question.

We don't believe in silver-bullet solutions. But we really like the fact that Clojure includes a client-side story.


## Skating to Where the Puck Is Going To Be
> I skate to where the puck is going to be, not where it has been.
> -- Wayne Gretzky

Clojure will help us fill in gaps for creating specific kinds of applications. Ones that need to be reactive or process high volumes of data. We're already in the era of multiple CPU cores, but our traditional languages haven't kept up with these computer architectures. Functional programming offers a new paradigm that allows us to more easily write programs that use all of that architecture. Clojure provides tools that will allow us to process large volumes of data efficiently. It lets us stream and transform data in near realtime. These features allow us to solve problems more effectively than we could solve them before.


## But Not Bleeding Edge
> “What’s known as bleeding-edge technology,” sez Lucas. “No proven use, high risk, something only early-adoption addicts feel comfortable with.”
> ― Thomas Pynchon, Bleeding Edge

At the same time, Clojure was released over five years ago and seems to be gaining good mind share in the developer community. It's not so early on the adoption curve that we're cutting ourselves on the bleeding edge. Clojure leverages existing technology like the Java Virtual Machine (JVM) that have been developed over the past 15-plus years. It can take advantage of the many high-quality Java libraries that already exist.


## A Little Something New Keeps Us Excited
> I haven't been this excited about learning a new technology since I learned Ruby!
> [-- Michael Guterl](https://teamgaslight.com/people/michael-guterl)

At Gaslight, we want to be able to attract and retain the best technologists in the industry. To do so, we need to offer people opportunities to learn and advance their skills. Clojure helps us offer that opportunity. It's a new paradigm with new sets of tools. But more importantly it allows us to tackle new and interesting problems. And nothing gets us more excited than having interesting problems to solve.


# More to Come
Want to learn more about functional programming? Join us for [Cincy Day of Functional](https://ti.to/gaslight/cincinnati-day-of-functional), a one-day conference on December 6. We expect to touch on Clojure, Haskell, Elixir, Erlang, Scala and more.  