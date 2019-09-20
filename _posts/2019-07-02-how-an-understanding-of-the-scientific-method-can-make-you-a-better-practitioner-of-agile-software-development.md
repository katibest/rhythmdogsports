---
layout: post
post_author: Jon Prell
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: How an Understanding of the Scientific Method Can Make You A Better Practitioner
  Of Agile Software Development
publish_date: 2019-02-18 13:30:00 +0000
feature_post_image: "/uploads/backlight-backpack-blur-837306.jpg"
post_images: []
slug: how-an-understanding-of-the-scientific-method-can-make-you-a-better-practitioner-of-agile-software-development

---
Here at Gaslight, we pride ourselves on being practitioners of agile software development, which we believe provides the best methodologies and practices to develop the best software. A core methodology we practice (and preach) is test driven development (TDD), which is a style of programming that uses testing to drive software design. The cycle is relatively simple: after choosing a story to work on next, the developer will write a test case for that piece of software they’re going to create. This test will fail initially as the code it’s testing is non-existent (or doesn’t return what is expected). You’ll then write the simplest code possible to make the unit test pass, and refactor your code before moving onto the next story which will require another test and restarting this cycle. An colloquial method to phrase this cycle is: red, green, refactor.

**Test Driven Development:**

![](https://gaslight-blog.s3.amazonaws.com/how-an-understanding-of-the-scientific-method-can-make-you-a-better-practitioner-of-agile-software-development/tddcycle_small.png)

As software craftsmen, we are always looking to better hone our skills within our trade, which includes developing a better understanding of even our most common practices. Applying the principles of the scientific method is one such way to ensure we’re executing the intent of TDD. If you’re like me and haven’t even thought of the Scientific Method since the days of tri-fold projects in your middle school science fair, you might need a slight refresher. It is a process for experimentation that is used to explore and answer questions. It is not a rigid set of steps, but more of a guiding principle for a researcher or scientist to craft an experiment that will stand up to public scrutiny and the results of which be generally or commonly accepted. 

The basic elements to the scientific method are the theory, the hypothesis, and the experiment. A scientific theory is a well-substantiated explanation of some aspect of the natural world. The hypothesis is an explanation for how an aspect of a theory works. An experiment is the procedure with which we apply to the theory to support, refute, or validate our hypothesis. The scientist will make observations about a theory, formulate their hypothesis, design an experiment to test that hypothesis, run that experiment and analyze the results, then refine the theory as necessary. A usual scientific experiment cycle will look like this:

**The Scientific Method:**

 ![](https://gaslight-blog.s3.amazonaws.com/how-an-understanding-of-the-scientific-method-can-make-you-a-better-practitioner-of-agile-software-development/scimethod_smaller.png)

It’s important to understand that a scientific theory is expected to undergo minor or even major changes in order to match the results of the experiments. In software development, the design of our software is analogous to the scientific theory. Our software will be refactored as a direct result of our experiments (the TDD process) and well crafted hypothesis (our test assertions).

Already you can see how these steps might be analogous to the test driven development process, but let's dive a little bit deeper. A major concept we must first understand is falsificationism, which is a scientific philosophy based on the requirement that hypotheses must be falsifiable in order to be scientific, i.e. if a claim is not able to be refuted it is not a scientific claim. This makes a lot of sense in TDD: you can’t write a test if the test cannot fail.

Next, there is a common misconception that scientific experiments are created to prove the theory is correct. This couldn’t be farther from the truth - in fact, an experiment is created with the goal of finding discrepancies with the theory. A scientist isn’t attempting to prove the theory works as they believe it should, but that it doesn’t. Science progresses by disproving a hypothesis rather than proving anything. A scientific theory is only generally accepted as it is refined by the results of experiments that withstand public scrutiny by the scientific community. The more a theory passes these experiments (tests), the more confidence the community has in that theory.

This same principle applies to software testing - just because the tests pass doesn’t mean the software works, but that the software works despite our assertions on how it could fail. It builds confidence that the software or program will work as intended, assuming the tests were properly and methodically crafted. It’s very difficult (if not outright impossible with software at scale) to test every potential variable or case, especially with non-deterministic factors such as such as concurrency or how to measure non-functional requirements. Therefore, no amount or quality of testing can assure you that the software will always work as designed.

The journey from making observations to crafting an experiment is a matter of identifying the best aspects of the theory to test, based on the potential value of the outcome. As we mentioned before, an experiment is designed in a way to disprove the theory, not validate wishful thinking. Therefore, any experiments that are already consistent with the current model of the theory provide little information. The same is true for determining what test to write next. It’s imperative we understand what we’re asserting and whether or not a previous test has already made the same or similar assertion.

What makes a good scientific theory is whether or not the experiments used to design the theory are generally accepted within the scientific community. This is accomplished through peer review and reproducible experiments. The same holds true for good software design. Besides automated test suites to ensure our tests are reproducible, agile software developers practice paired programming to help us leverage our developer’s strengths, share a common view of the software design, and avoid the pitfalls of poor testing, such as internal bias, misinterpreting data, or ignoring results that contradict our ideas about the software. 

The scientific method is a proven process that helps us better understand the natural world around us. When applied to agile software development, the scientific method can help us reinforce the value of these practices by providing a different perspective through which to think about writing software. There is a strong correlation between bad science and poor software design. Both result in speculative theories/software that cannot be reinforced by good experiments/tests, and are not trusted by the community at large. This demonstrates that agile and test driven development provide the best value for building quality software. 
