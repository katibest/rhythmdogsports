---
layout: post
post_author: Michael Guterl
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Mitigating Risk Through Technical Spikes
publish_date: 2013-12-17T16:10:00.000+00:00
feature_post_image: ''
post_images: []
slug: mitigating-risk-through-technical-spikes

---
Our customers frequently come to us with an idea and, under tight time constraints, we need to validate that we can develop a component using a technology that we're not entirely familiar with. It is impossible to keep up with all of the technologies out there and at Gaslight we pride ourselves at being able to integrate new technologies quickly.

We don’t want to make any promises we can’t keep. Undertaking projects with bad assumptions can turn out badly. It can be very unsatisfying for a client, very damaging to our reputation, and very expensive for both of us. This is an inherent risk in development. Focused technical spikes are a great way to mitigate this risk.

Recently a customer came to us looking for help converting an existing application to [Ruby on Rails](http://rubyonrails.org). Their timeline was very compressed and they needed us to move very quickly. We needed to evaluate several options for creating reports. We could automate the creation of existing Excel reports or try to satisfy the client's needs with PDF documents generated from HTML.

The underlying data for the spreadsheets was not too complex. However, the reports used several advanced Excel features that we had never automated before. Ruby has [plenty of libraries for reporting](https://www.ruby-toolbox.com/categories/reporting), we just needed to find the one that fit our needs.

## Using a Spike

From [Extreme Programming](http://www.extremeprogramming.org/rules/spike.html):

> Create spike solutions to figure out answers to tough technical or design problems. A spike solution is a very simple program to explore potential solutions. Build the spike to only addresses the problem under examination and ignore all other concerns. Most spikes are not good enough to keep, so expect to throw it away. The goal is reducing the risk of a technical problem or increase the reliability of a user story's estimate.

A spike solution can vary in size greatly depending on what you hope to explore and achieve. [Extreme Programming](http://www.extremeprogramming.org/rules/spike.html) mentions putting a pair of developers on a spike for a week or two. Seeing that our project has a four week timeline, this would not have been appropriate. We decided to timebox [link] our spike to two hours.


## 1. Define a goal

The most important thing to do when spiking a solution is to define your goal. Defining a goal provides focus. Without focus, you’ll waste valuable time doing too much. Remember, you’re singular purpose is to vet a solution. You want to gain confidence that you can accomplish something, not actually accomplish it. Remember, code developed during most spikes is meant to be thrown away. If you find, given the constraints of the project, that you can’t accomplish a task, you’ve done your client a huge service.

Our goal was to determine if we could use conditional formatting inside of Excel through [axlsx](https://github.com/randym/axlsx). Lucky for us, [axlsx](https://github.com/randym/axlsx) is very well documented and contains a lot of [examples](https://github.com/randym/axlsx/tree/master/examples). While it did not cover all of our needs, there was an even an example for [conditional formatting](https://github.com/randym/axlsx/blob/master/examples/conditional_formatting/example_conditional_formatting.rb).

## 2. Limit your scope

Just because your spike isn’t meant to deliver production ready code, doesn’t mean you shouldn’t start with a plan. The more you intentionally you limit the scope of your spike from the beginning, the faster you can move towards your goal. What is really important? What knowns can be faked out, simulated, or stubbed to more quickly vet the unknowns?  

As a very critical person, I'm always looking back and trying to see how things could have gone wrong. Our spike could have quickly outgrown our two-hour timebox by trying to do too much. Instead of working outside of the application with a constant / independent data set, we could have tried to integrate our work into the applicaiton. Integrating with the existing application would have required understanding the data model. We could have also lost focus and tried to actually implement the entire report, rather than ensuring that we could achieve our main goal, conditional formatting.

## 3. If the goal is too broad repeat, if not evaluate the solution

The high-level goal for us was to determine if we can build an Excel reporting component. After we determined that we could in fact write out Excel files, we then had to determine if we could use conditional formatting. Once we found a library that supported conditional formatting, we needed to determine if we could use it for our purposes.

By ensuring that we had a small, focused, and independent goal we were able to make sure that [axlsx](https://github.com/randym/axlsx) was able to meet our needs. Doing so allowed us to move forward with greater confidence that we could meet our customer's deadline.