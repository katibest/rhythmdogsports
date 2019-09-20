---
post_author: Joel Turnbull
categories:
- Development
tags: []
post_title: How to Time Travel Your Data
publish_date: 2013-07-17T15:00:00.000+00:00
feature_post_image: "/uploads/time-travel-your-data.png"
post_images: []
layout: post
current_gaslighter: false
slug: how-to-time-travel-your-data
permalink: "/blog/:slug"

---
I wanted to get my thoughts down about a recent talk I gave here at the Gaslight Office about [Datomic](http://www.datomic.com/). It was inspired by a [talk](http://www.youtube.com/watch?v=1E_n47ct280) given at RailsConf 2013 by Yoko Harada introducing Relevance's [Diametric Gem](https://github.com/relevance/diametric), which provides an ActiveModel wrapper to Datomic.

Datomic is database written in Clojure and designed by Rich Hickey. I haven't scratched the surface of what it can do, but the intriguing aspect of Datomic for me was the immutable nature of the data, and what that could do for you.

One of Clojure's fundamental concepts is immutability of data. The way I understand immutability is, information is always added, never replaced. When you change data in an immutable object, you get a new copy of that object with new state. However it's not necessarily a full copy. The new object points to the old object and masks it's state with new state, like an applied diff.

What this means is that you have a record of state that travels back to inception. For data, this seems very powerful. We have services like Apple's Time Machine where it's easy to see the idea of "version control" for data in practice, but this concept is foreign in the databases that I use on a daily basis. Imagine a SQL table in three dimensions.

In my talk, I gave the example of a markup percentage. I worked a on a web application in which a markup percentage was stored in a column in the database for each customer, and was applied to every cost a customer saw on the screen. They were charged those costs on a monthly basis. At a 30% markup, a cost of $1000 shown for July was $1300. In August the markup for that customer was reduced in the database to 20%. Costs shown for July were now $1200, when what the customer actually remembered being charged in July was $1300.

Simple example of a common problem. How do you store the value of a piece of data over time? What was the value of markup percentage in July? What was the status of this order three days ago? What did this property cost in 2011?

There are products out there that preserve history and store versions of data. CouchDB, for example. In fact, for the use case of my simple example, that would work just fine. Rails has extensions like [acts_as_audited](https://github.com/collectiveidea/acts_as_audited) that we've used with some success. Audit tables back model tables, storing attribute changes as data structures. But what happens when you need to query and compare the history of more than one model or document? The distinction is that with those solutions, you deal independently with the histories of disparate pieces of data. With Datomic, you deal with the historic state of the database as a whole at a moment in time. It's simple.

The idea of querying based on time is not new. Micheal Guterl pointed out to me the term "bitemporal databases". There are seemingly a few out there, and a bitemporal package for Postgres. My feeling is that Datomic might provide the easiest and most elegant solution to getting that type of temporal information. In Diametric I watched Yoko pass a time object to the database and then query it in it's entirety "as_of" that moment in time. It's baked in. Data is always preserved, the past never changes, and time is always something you can query on easily.

I'm a fan of writing about things I have no practical knowledge of. I can't help but think that this concept of time  should be inherent in modern databases. I'm interested in hearing thoughts and experiences on Datomic, Diametric, bitemporal databases, or querying state over time.
