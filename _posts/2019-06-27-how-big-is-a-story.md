---
post_author: Joel Turnbull
categories:
- Development
- Business
tags: []
post_title: How Big is a Story?
publish_date: 2013-06-24T13:38:00.000+00:00
feature_post_image: "/uploads/obama.png"
post_images: []
layout: post
current_gaslighter: false
slug: how-big-is-a-story
permalink: "/blog/:slug"

---
In conversations about Kanban, @dougalcorn mentioned [Jeff
Patton's](http://www.agileproductdesign.com/) excellent article [Kanban Over
Simplified](http://www.agileproductdesign.com/blog/2009/kanban_over_simplified.html).
What was specifically interesting about this article to me was [The mystery of
the shrinking
story](http://www.agileproductdesign.com/blog/the_shrinking_story.html).

You might be surprised that the term "story", as described by Kent Beck in the
first edition of Extreme Programming Explained, was characterized as "estimable
between one to five ideal programming WEEKS." This was shocking to me, as it
would be to most developers. Not so long ago, here in the office, we would say
that if a story was on it's second day, it needed some help, and if it was on
it's third day, there was something wrong! I imagine that most dev shops are
similar.

What caused this amazing shift in story size? There are a few reasons.
Customers, in order to plan and assess risk, want estimates, and smaller stories
are easier to estimate. It's also natural for developers to fall into the trap
of seeing the backlog as a todo list of tasks that need to be completed. I also
think one of the main reasons stories have become so small is to maintain the
illusion of momentum. It's about developer happiness, and the satisfaction that
comes from clicking "Start, Finish, Deliver" on a daily basis. I'll talk about
why that's an illusion later, but I personally feel that maintaining that
illusion is beneficial and maybe even crucial to a successful project.

What is wrong with small stories? They make the backlog longer, and small
stories are harder to reason about and fit together into anything cohesive. But
the real detrimental effect of small stories is loss of focus on the customer's
needs. Small stories lean more towards up-front planning and implementation,
rather than customer goals and values. The illusion of momentum is that forward
progress is being made, when no value is being delivered. It is not uncommon to
break a feature down to such a detail of implementation, that the product owner
is asked to click "Accept", having no confidence that anything was actually
done. Asking a customer to "take our word for it" is fail.

Don't get me wrong, planners should still strive to make stories as small as
possible. However, stories must first and foremost meet a standard of minimum
marketability. A minimally marketable feature is "the smallest possible set of
functionality that, by itself, has value in the marketplace". Meeting that
standard is not always easy to do in a few days of development.

Most importantly, stories that the customer sees should clearly define value.
The most effective way of doing this is to write a story using the format
described in Cohn's [User Stories
Applied](http://www.amazon.com/User-Stories-Applied-Development-Addison-Wesley/dp/0321205685)
"As a [type of user] I want to [perform some task] so that I can [achieve some
goal]".

But what about developer happiness and momentum? Isn't it demoralizing to be
working on delivering one piece of functionality week in and week out? Isn't it
satisfying to click "Start, Finish, Deliver" on a daily basis? I would argue
that it's equally as important for developers, behind the scenes, to continue
striving to deliver subsets of functionality on a daily basis, and to track
that. I think we have a lot to learn about how to achieve that and also focus on
customer value.

In our previous article, [Why Not Pivotal
Tracker](http://blog.gaslight.co/post/52640678160/why-not-pivotal-tracker), Doug
states correctly that velocity is flawed. I still think that the illusion of
velocity is a beneficial force from a developer standpoint. Velocity is a lie
that deceives a customer, but motivates a developer. Perhaps we need new
terminology to deal with that separation of concerns. One thing is clear, first and
foremost, "stories" should define and communicate value to the customer.
