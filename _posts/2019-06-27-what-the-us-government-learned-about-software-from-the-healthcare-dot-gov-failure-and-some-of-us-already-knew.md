---
layout: post
post_author: Joel Turnbull
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: What the US Government Learned About Software From the Healthcare.gov
  Failure (And Some of Us Already Knew)
publish_date: 2013-12-12T18:24:00.000+00:00
feature_post_image: "/uploads/obama.jpg"
post_images: []
slug: what-the-us-government-learned-about-software-from-the-healthcare-dot-gov-failure-and-some-of-us-already-knew

---
## Part One: The Bad

On October 1st, 2013 the U.S. Department of Health and Human Services released the Healthcare.gov website. The portal was intended to draw and enroll an estimated 7 million users over a six-month period. The release was a huge failure. Problems with the site were so bad that by December, two months after the release, a reported 29,000 users had succeeded. Users reported a myriad of failures in accessing the site, authentication, bad calculations, bad and missing data, and dead-end applications during the enrollment process.

What happened with Healthcare.gov can and does happen with many software projects, but it doesn't have to. More often than not, technical problems like scalability are not the reasons for failure. Software projects are fragile and fail due to poor management and poor engineering. Since the fiasco, the blog of the Department of Health and Human Services has belied a snowballing improvement by shifting towards Agile development and management practices. This is a combination of articles about how poor project management practices killed the Healthcare.gov project, and how smart practices have begun to turn it around.

## Why Software is Hard

The release was a huge black eye. This happens in businesses everyday. Software projects are fragile and can fail easily because they are complex. They are complex because:

- Requirements of the project are allowed to change
- The owners of projects aren't able to anticipate exactly what the project will require until they use it
- The developers of the software can't anticipate the best solutions until they develop them.


These complexities are established as laws in the software development world. We can't change them. The practice of good software project management has formed around accommodating, accepting, and embracing these complexities, not ignoring or fighting them.

Everyday we learn more about how to manage software projects. Two methodologies of effective software management that have emerged are Lean and Agile. Lean deals with efficient discovery and vetting of value before pouring in development resources. Agile deals with delivering value iteratively and flexibly through engagement, feedback, and responsible development practices.

There is no doubt that the Healhcare.gov initiative is a very large and complex project. Implementing it would be a challenge for even the best teams. However, reports following the disaster are rife with examples of poor, outdated, software development practices. Here are just a few of those problem indicators I found. 

## Lack of Engagement 

"By late summer, teams of agency officials had parked themselves in CGI Federal’s headquarters in Herndon, Va., demanding on-the-spot reviews and demonstrations of new code that was never tested." [Source](http://www.nytimes.com/2013/11/23/us/politics/tension-and-woes-before-health-website-crash.html)

Why did it take until late summer for agency officials to see features? Why were they pounding on the door at the midnight hour? Applications developed in a black box fail. This is because we learn things during the process that change the direction and goals of the project. Feedback and flexibility are key, and engagement between the owner of the product and the development team essential.

Agile development teams aim to engage product owners and solicit feedback as often as possible. That is because engagement is one of the most essential factors for determining the success of a project. Agile methodology encourages a culture of daily stand-up meetings and regular planning sessions attended by the both technical and business stakeholders of a project. Agile practitioners release often and require product owners to sign off on what they deliver regularly. The more engagement, the better. Our personal recommendation, especially in cases where the timeline is tight, is that the product owners embed themselves with our team in a shared physical space. Instant feedback is the best kind, so set the expectation for engagement with product owners from the beginning.

## Lack of Flexibility

"Others point fingers at the Department of Health and Human Services, which took years to issue final specifications, preventing CGI from really getting started until this spring." [Source](http://www.washingtonpost.com/blogs/wonkblog/wp/2013/10/16/meet-cgi-federal-the-company-behind-the-botched-launch-of-healthcare-gov)

In the world of modern software development, the excuse of no final specifications carries no weight. There is no such thing. Requirements can and should shift substantially. Any development initiative should be prepared to invite and accommodate these shifts.

Agile practice focuses on fast iterations. Features delivered in an iteration of 1-2 weeks should contain functionality that is deemed most valuable at that time. What is considered valuable, and what is considered a requirement, will change frequently when revisited on a weekly basis. Teams who attempt to anticipate and solidify requirements at the onset of a project and disappear for six months end up delivering something that their client didn't really want and can't use. We've learned that it is impossible to anticipate or communicate every requirement for a software project. Something is always left out, assumed, or unknown. Embrace this tumultous aspect of software development.

## Lack of Agile Engineering Principles

"I've got an application right now that's like half done, but I have four copies of my wife in it and can’t get rid of it and can’t start a new application," says James Turner, a software engineer and former systems administrator who wrote a critique of Healthcare.gov. "It's pretty clear that they didn’t do a lot of testing, because it's easy to get into states where you can’t get any further or you have bogus data you can’t get rid of." [Source](http://www.theverge.com/2013/10/8/4814098/why-did-the-tech-savvy-obama-administration-launch-a-busted-healthcare-website)

Stories like this provide an insight into the team's disdain for thorough testing. The nature of change in software assumes a risky ripple effect. Code changes have unforeseen side effects and consequences in complex codebases. Every feature should be considered a risk, every line of code considered a liability.

What happened in the scenario above? As each step in the enrollment process was coded and rolled in, lack of testing failed to indicate bugs, or regressions, that were introduced into the existing functionality of the system.

Developing a suite of tests that accommodates a codebase and drives feature development is a primary tenant of Agile engineering practice. Test suites provide a safety net that mitigate the inherent risk of software change. Covering each feature with a runnable test continually exercises the existing functionality of the system in an automated, repeatable way. Test failures indicate abnormalities introduced by new features, and quickly point out the offending lines of code. Tests provide a basis of confidence in the codebase. They allow for rapid and focused development, resulting in flexible and maintainable code.

## Lack of Planning

"Unfortunately, in systems this complex with so many concurrent users, it is not unusual to discover problems that need to be addressed once the software goes into a live production environment" [Source](http://america.aljazeera.com/articles/2013/10/24/healthcare-web-sitecontractorsfacecongress.html)

No team immersed in a codebase for months at a time can anticipate all of the strange ways that real users can break a system after it's been set loose on the world at large. One giant release is a recipe for disaster. The goal of Lean is not to release a perfectly complete system. Experienced teams embrace the fact that this is impossible. The goal is to mitigate fallout by releasing intelligently.

Again, feedback is key. It should be gathered on a continual basis. The product should be put in front of users as soon as some minimum set of features has been developed. Real-world releases should happen gradually to a subset of users so that inevitable bugs can be worked out, before releasing to the next set. We have all become familiar with the term "beta". Most of us have given our email address to an online service, expecting to get an invite sometime in the future. These are smart release practices that mitigate frustration, and set a realistic expectations that don't damage reputations.

### Procurement Problems

"There are also a number of extra requirements for government contracts that made state-of-the-art, Silicon Valley-style development impossible for this scale of government project. For example, contractors that worked on the Healthcare.gov backend were probably required to have Federal Information Security Management Act certification, which rules out the smaller, more agile firms that could have brought a more innovative sensibility to the project." [Source](http://www.theverge.com/2013/10/8/4814098/why-did-the-tech-savvy-obama-administration-launch-a-busted-healthcare-website)

Government procurement problems are a little outside the scope of this discussion, however I included this to highlight what may not be obvious to those outside of our industry. There is still a palpable divide between the old and new worlds of software development. For every company doing it right, there are many still doing it wrong. These aren't just buzzwords. The most respected technology companies in the private sector have been studying, practicing, and refining their Agile and Lean methodologies for years, and are proving their effectiveness. 

Custom software development is hard. There are still more questions than answers. Good teams are constantly striving to improve by integrating ideas from diverse and disparate management methodologies. We've been experimenting with [Kanban](http://gaslight.co/blog/why-not-pivotal-tracker), a flexible process management system from the world of manufacturing. 

Software development is a unique endeavor. Success requires a unique and evolving set of skills and experience that only some companies are eager to adapt.

## Conclusion

I've outlined some of the apparent problems from a modern software development standpoint that led to the Healcare.gov fiasco. In my next article I'll talk about the smart things that the government has done since to turn the ship around. Stay tuned.