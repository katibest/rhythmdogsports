---
layout: 'post'
post_author: Peter Kananen
categories:
- Development
tags: []
post_title: Better Code Design through Pictures
publish_date: 2013-09-25T13:20:00.000+00:00
feature_post_image: ''
post_images: []
current_gaslighter: true
slug: better-code-design-through-pictures
permalink: "/blog/:slug"

---
When I switched from Java development with Eclipse to Ruby and Rails with Vim, one part of my workflow that I missed for quite a while was the visual file browser in my IDE I was so used to seeing. It turns out having a visualization of the directory structure really helped me understand the project at a higher level. The relationship between all of the files and folders is just not very obvious when looking only at one or two of them at a time.

Something similar can be true for the code we write. If you subscribe to agile practices and use TDD religiously, you probably aren't drawing pretty pictures outlining your class hierarchy or proposed architecture. These sorts of diagrams are usually seen as relics of the era of Big Design Up Front, or perhaps something like the Rational Unified Process. Many developers may write entire applications without ever looking at their class structure or project architecture outside of their editor, or perhaps from RSpec/Cucumber output. As we've been reading through [GOOS](http://www.amazon.com/Growing-Object-Oriented-Software-Guided-Tests/dp/0321503627), we've noticed the authors frequently employing state chart, class hierarchy, and sequence diagrams. These didn't appear to just be for the sake of explaining the domain to the reader - it seemed clear these diagrams were used to help understand and even drive incremental design decisions.

During some recent refactoring and redesigning on a project, we decided to sketch a few diagrams to better understand the system in its current state, and what changes would make most sense. We spent some time looking at the class hierarchy and dependencies. Here's what we came up with:

![diagram](http://gaslight.github.io/posts/assets/images/diagram.jpg)

Looking at a picture like this reveals so much that is missing when only looking at Emacs or Vim. Classes that violate the Single Responsibility Principle may become obvious because they're related to too many other classes. Cyclical dependencies might be identified. Even class names may be brought into question. These discoveries are not very obvious when writing code, but they were remarkably obvious once we threw the structure up on the whiteboard. Drawings like this also spur conversation between team members, which tends to promote creative problem solving even more strongly. It became apparent that more frequent white-boarding and intentional architecting could help us accomplish rather complicated refactoring and redesign efforts in a shorter amount of time, and with more clarity.

Our ability to come up with solutions to problems is much easier when we look beyond the editor, draw pictures, and talk about them. Next time you're unsure of the best way to delineate responsibility between classes, introduce a new dependency, or simplify your application's architecture, it's worth your time to break out the markers and draw some pictures.
