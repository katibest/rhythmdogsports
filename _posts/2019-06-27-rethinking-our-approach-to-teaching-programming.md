---
layout: post
post_author: Kevin Rockwood
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Rethinking Our Approach to Teaching Programming
publish_date: 2014-02-13T18:21:00.000+00:00
feature_post_image: "/uploads/classroom.jpg"
post_images: []
slug: rethinking-our-approach-to-teaching-programming

---
Since January, @too_mitch and I have been teaching a Ruby On Rails course at Northern Kentucky University. It's been a challenging and rewarding experience, and it's prompted me to rethink our approach to teaching students how to program.

## The Problem With Traditional Programming Education

It wasn't long into my first "real" programming job before I realized that my college courses really didn't prepare me for real-world programming. For the most part, my college courses focused on the theoretical rather than the practical side of programming. Things that are fundamental to everyday programming, things like testing and source control, were rarely discussed.

Part of this sentiment is summarized in the post ["Goodbye, s***ty Car extends Vehicle object-orientation tutorial"](http://lists.canonical.org/pipermail/kragen-tol/2011-August/000937.html). I often find that students have taken object-oriented classes but don't know how to apply these ideas to manage complexity in an application. Students have taken web programming classes but don't know what a session is.

With this class, Mitch and I wanted to give students exposure to a more sustainable and realistic learning process.

## Less Talk More Walk

In each class we give a short lecture on all the concepts students will need to tackle the next assignment. The funny thing is that it doesn't matter if we tell students every step to take to complete the next assignment, they don't internalize the concepts until something stops them from building their app. Only once they hit a wall they begin to understand the ideas we talked about and ask questions. We could lecture all day about how database migrations work in Rails, but until students destroy their databases in many exotic ways they can't understand the how of and why of the process.

## You Made Your Bed...

Often computer science classes have students work on many small assignments in order to focus on specific concepts for the course. A couple times a week students write 100 lines of code and throw it away. However, working programmers do not have this luxury. When they write code they must live with it, refactor it, and modify it.  If you don't live with your code, you can't appreciate design.

Each week we have students build on their work from the week before. This lets students build a substantial application that does real work and it gives them a sense of ownership over their code. As a result we often see students asking how to implement features beyond what is required for the assignments. Students were frustrated that after they signed up a new user in their app, the user still had to login. They wanted to know how to "autologin" their users on sign up. About half of the class added stylesheets and are using Twitter Bootstrap to make their apps look more professional.

The flip side of this long-running app process is that when students implement some gnarly code in a controller, they often suffer when trying to implement new features. We do our best to guide them towards better designs, but nothing we say can teach them more than wallowing in the code that they have written.

## Real World Workflows

I know one class at the University of Cincinnati where students write programs to complete math problems. When they finish an assignment, they print their code on paper along with some sample inputs and outputs to hand into their teachers. One goal for our class is to provide a more realistic workflow for doing programming work.

On day one, each student created a Github account and forked our class repo. Each student codes on a dedicated Nitrous.io box, and they push their code back to Github. When students run into problems, they create Github issues.

Students usually answer each other's questions on Github issues before we can get to them. On the whole, answers given by students are as good as what the instructors could have given. Students soon realized that if they kept their Github repositories up-to-date, other students could review their code and comment on it.

## The Spirit of Adventure

Mitch and I both have experience teaching in some capacity, but the university setting is new to both of us.  As opposed to professional programming workshops, students in a university setting spend a lot more time without any instructor. Assigning grades to students is a new concept for us as well.

There's no doubt that many students find the class more challenging than their other coursework. The style has a polarizing effect where some students thrive on the excitement of building software where others often comment that they don't know where to begin. We struggle to find ways to help those who fall behind, but for now I'm satisfied that we've created a course that captures the spirit of building software.