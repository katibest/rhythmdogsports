---
layout: post
post_author: Tim Kindberg
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: How to Succeed in Open Source ( In Ways You Haven't Considered Yet )
publish_date: 
feature_post_image: "/uploads/tim.jpg"
post_images: []
slug: how-to-succeed-in-open-source-in-ways-you-havent-considered-yet

---
Tim Kindberg is a front end developer and UI designer here in Cincinnati. He's recently [presented Angular UI-Router](http://www.youtube.com/watch?v=dqJRoh8MnBo) here at [cincijs](http://blog.cincijs.com/) and has helped us spelunk some of our Angular code. When Tim started talking about open source it was obvious that he had a unique experience. New contributors to open source projects can get cold feet, but contributing doesn't always mean writing awesome code. Tim presents a way of contributing that is both novel and highly valuable.

-----

![Octocat](http://gaslight.github.io/posts/assets/images/octocat.jpg)

## It’s Easy to Feel Entitled in the Open Source World

A while back, it was easy to think of open source projects as real products. I think that I thought the contributors of these projects were getting some sort of kickback. I’m not sure what I thought they were getting—I assumed that they were getting something and by golly they better work for it! Probably just prestige points that they could trade for sweet jobs, contracts, instant seating at hot spots. Maybe they’re mega-geniuses whose duty it is to share their insight with the world before they die and take that glorious javascript library with them. That is their duty!

I wasn’t necessarily the type to storm into a github repo and demand a new feature, I can’t remember if I’ve ever stooped that low, but I remember feeling as though these “products” ought to be run with the utmost professionalism. I want bug-free code, a dead simple API, changelogs, clear documentation, and while you're at it give me a cool website so I can get excited about using your free thing! Its not that I thought through any of this in detail, it was just sort of an expectation that lived below the surface of my psyche.

I don’t think that way anymore. I’ve learned to be extremely appreciative and patient with all of my favorite libraries and frameworks. I’ve learned that open source projects are hard to do right; like really right. Its hard to do a paid project right for that matter. So for those OS projects out there getting it right, I applaud you! Bravo!

And the best way to really learn to appreciate open source and how it works, the only way you can really take a look behind that curtain, is to contribute. Go get a Github account—if you don’t already have one—and contribute.

## How I Succeeded By Failing at Open Source

I personally got more involved in open source by writing the [documentation for UI-Router](https://github.com/angular-ui/ui-router/wiki). The short version is that I was researching Angularjs and Emberjs as frameworks. I preferred Angular overall but I liked Ember’s router. I wanted Embers routing capabilities to exist for Angular. After some searching, I found out that it didn’t exist. How dare the internet not supply me with a thing!!!

I did find a budding online discussion though. The Angular-UI folks were talking about this very idea of nested routing. Hmph! Well I guess I have to participate in this conversation. And I did. I cared enough to really stick with it too. I reviewed code submissions, I wrote down my thoughts and eventually we all came to a consensus for an API. Wow, I just helped a community move forward with a solution. Many thanks to [Karsten](https://github.com/ksperling) for the initial code dump that comprised 0.0.1 and to [Nate](https://github.com/nateabele?source=c) for keeping the code alive almost single-handedly. 

It was a great feeling. I wanted to help more. But that ui-router code is scary and those other guys seem to know it so well and… they are just better than me. I didn’t care. I decided I was going to write the docs. This module would need documentation, it would need an API reference, and a FAQ and of course a Readme. Writing docs is not for “grunts” or “bitches”. Its not an easy task to organize thoughts—especially technical ones—into an easily digestible format. Documentation can make or break an open source project, or any project for that matter (look at Ikea!).

So I wrote them. I didn’t do a good job. Boy did I miss the mark! My docs initially created more confusion than they were supposed to alleviate. I wrote examples incorrectly and just plain lied about some of ui-router’s capabilities because I misunderstood how it worked. But my effort led to action. And some docs were better than no docs. Users started to point out the problems in my documentation (aka the action) and I was able to refactor them over time. It’s easier for people to point out the problems of work that exists than to tell you what work needs to be done. Eventually the docs became a life-saver and a daily check-in point for users of UI-Router. I’ve had people tell me thanks; that they depend on my documentation daily; that it’s well organized. That feels good.

## Unsure of How to Help Out?

Contribute your time. It really is the most valuable thing you can give. Beyond that, contribute anything! Whatever is visibly in need of being worked on.

### 1. Write Documentation
Did you find a typo in the docs? Consider fixing it. It really helps the project showcase itself more professionally.

Or maybe you found that the guide conveyed an idea ineffectively. Try taking a stab at fixing up the errors, or rewriting the confusing bits. My friend, Jim, told me that he’d gone in and fixed up some errors that he found in the guide I wrote, and I’m glad he did. Because the truth is, most of the time I don’t want to work on the project. I want to chill with my family, enjoy a book, or just generally turn off the noise from the day. That’s why doing open source is hard. It’s more time you have to sit at a computer. But this article is trying to show you how its not a lost cause with no ROI. There can be incredible, life-changing returns from helping with open source projects. We’ll get to that in a bit. First what are some other ways you can help out?

### 2. Report Bugs
Did you find a bug in the behavior of the library/framework? I implore you to write it up. It’s easy to feel that someone else will write the bug, and they may, but I challenge you (and myself) to hear this voice next time you come across your next defect: “Don’t even think about not writing it up. What are you? Would you leave a toilet unflushed? Gross man. You are never too busy to write up a bug for a project that you are using for free. Well maybe you are, but try to at least make a note and write it up later.”

### 3. Fix Bugs
Did you fix a bug?! Holy shit, digital high five! You’ve already done so much. Submit a pull request asap! If you are still scared of git, then at least write up the bug along with how you fixed it. Just mention that you're a git newbie, no worries. I honestly don’t think anyone knows git, we all just pretend that we do. 

### 4. Request Features
Want a new feature? Write up an RFC, which stands for Request for Comments. An RFC is basically your idea in full; how you perceive the current shortcoming, how your idea solves it, preferably with some psuedo-code examples, and finally if you are capable, volunteer to implement the feature once a consensus is found. Trust me, no one cares about that feature more than you. The RFC will help you find others who agree with your idea and they can give you feedback and perhaps even volunteer some of their time too. [Here’s a great RFC](https://github.com/angular-ui/ui-router/issues/562) we got recently.

Most of all, don’t be shy. Don’t worry about doing it wrong. Some code on the table is better than no code on the table. Your effort will translate into action. If you put your effort into something then its more likely that the core contributors of the project will support you. Or maybe they’ll hate your idea, but then you’ll all know that’s one idea that won’t work. And you can focus on a new one that might work. Its still progress. Doing it wrong is actually doing it right. Here’s a great quote by Addy Osmani, “First do it, then do it right, then do it better.”

## More People Who Won By Writing Docs

Getting back to that ROI thing. I’ve found success from being attached to the ui-router project, in particular writing good documentation. Having that under my belt has opened up so many opportunities to me and helped to expand my network. It isn’t why I contribute, but I’d be lying if I said it wasn’t nice to get that payoff.

There are lots of stories of others who’ve experienced documentation success. Joel Hooks, told me his story. He got his start by writing the documentation for the RobotLegs framework for ActionScript. He stumbled onto his project just like me. After researching it he felt like he found a diamond in the rough. Here was this obscure yet beautiful project. In his heart he knew it needed documentation to become popular. Writing the docs for that one project kickstarted his entire career! He was propelled into an evangelism role for the project and started speaking at conferences (even though speaking was absolutely nerve wracking to him). Writing docs compressed his five year career plan into about one year. Ha, I knew there were kickbacks! He ultimately became a consultant and even wrote a book and now he also teaches important concepts online via [articles](http://joelhooks.com/) and [video tutorials](http://egghead.io/).

The key point is that he would have never written the RobotLegs framework—it was the stuff of legend—but he could write the fucking docs. I’m crying right now, it’s too beautiful. 

I also spoke with Matias Niemelä who found success in a similar fashion. He blogged for a while, mostly about MooTools, hence his site’s name [yearofmoo](http://www.yearofmoo.com/). He started to learn Angular and wasn’t really finding good learning materials on the web (back during the pre-1.0 days). So he made an article on SEO and it was really popular. So he wrote another article and the popularity continued, more and more. This caught the attention of the core team and somewhere between Matias’ third and fourth article Misko (an angular core team member) asked him if he’d be able to help revamp the Angular docs as well as create ng-animate as a contractor for Google. It paid off for Matias.

It paid off because he’s passionate about explaining things in a delicate way that simultaneously pulls incongruent yet pertinent details together into one place. He wanted to remove the need to jump from site to site to fully comprehend a single concept. I think if you look at his articles on [SEO](http://www.yearofmoo.com/2012/11/angularjs-and-seo.html), [Testing](http://www.yearofmoo.com/2013/09/advanced-testing-and-debugging-in-angularjs.html), and [Animation](http://www.yearofmoo.com/2013/08/remastered-animation-in-angularjs-1-2.html), you’ll agree that he’s achieved this goal. Even without his leadership role with ng-animate Matias says he’d still have been perfectly happy only working on documentation. He says “Devs notice what makes their lives easier and will notice good docs.” Matias hinted at a cool new boosted-up interactive guide page that is being worked on as well; should be sweet!

Lastly I chatted with [Pete Bacon Darwin](http://bacondarwin.com/), whom I’m still not sure if that’s his real name, if it is that’s awesome! His entire career is based on good vibes from open source. He started out by being helpful in the Angular google group, answering emails. He enjoyed helping, giving back to the community, and never expected anything from it. He was learning by helping. Well again, similar to Matias, the core team took note and asked if he’d like to continue helping officially as a Google contractor. I’m sure that was a no-brainer for him, and he’s even moved on to writing a [book for Angular](http://www.packtpub.com/angularjs-web-application-development/book) with his friend Pawel. 

So now he can spend more time doing what he was already passionate about doing. Pete genuinely thinks that most devs contribute to open source for the feeling of giving back. He acknowledges that there is a sort of prestige that comes with working on a well known project though. However I think an ego boost is a great reward for contributing; we all love to be loved.

Pete’s contributions to open source is a success story. He’s now positioned well in the community, so he gets offers often to consult, train, do code reviews and provide guidance.. He feels when you put effort in, you get it right back. That’s really the magic of open source and why its so successful. It complements (and sometimes augments) our real jobs and lives so well.

## Conclusion

As for me, I still don’t think my docs are perfect, but they are good enough. They support the beautiful module that they document. And that is the key, it doesn’t need to be perfect, and open source never is, nothing is! We are working on this together and we are working towards perfection.