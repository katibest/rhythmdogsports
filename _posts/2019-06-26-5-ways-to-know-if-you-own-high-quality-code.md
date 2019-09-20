---
layout: post
post_author: Kevin Rockwood
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 5 Ways to Know If You Own High Quality Code
publish_date: 2013-11-13T14:37:00.000+00:00
feature_post_image: ''
post_images: []
slug: 5-ways-to-know-if-you-own-high-quality-code

---
Do I have high quality code? This is often a subjective question, but there are some tools that can give a more definitive answer. Don't worry if you're not a developer. These tools are easy to use and don't require any coding!

## 1. Run CodeClimate

Codeclimate is a code assessment tool that we use to track code quality. You point CodeClimate to a Github repo and it will score each file and assign a GPA style score to your repo. It will track quality changes as your codebase grows and alert you to downward trends in code quality. It's certainly not a definitive measure of quality, but it's an easy way to find glaring problems.

## 2. Are there tests?

As your codebase grows in complexity, regressions bugs will start to popup as each new feature is added. We believe that a high-quality test suite is an invaluable tool to catch those bugs before they reach production. Writing tests also improves the overall design of a codebase by ensuring that objects are decoupled from each other. If your codebase isn't tested, it will get more and more difficult to maintain as time goes on. Check if your code has tests, and that they are run regularly.

## 3. How do your commits look?

A clean git repo is a good git repo. It's important that commits to your code are concise and easy to understand. When we roll onto a new project the first thing we do is read the commit history to understand how the code has evolved over time. You can look at the commit history of a repo right on Github or use a visual tool like Github for Mac or Sourcetree. Check that commit messages are well written and explain exactly what changed. Having 10 or more files change in each commit probably indicates a problem.

## 4. Print an ERD

An ERD (Entity Relationship Diagram) is a diagram of your database. It shows how the business objects are connected to each other. An ERD multiple paths between tables or duplicate table and column names probably indicates some issues with your database. There are many tools to create ERDs. We useRails-ERD for our apps.

## 5. Run New Relic

New Relic is a service that monitors your production application and finds problem areas in your code. New Relic will show where your app is slowing down. Often, just a couple lines of code can slowdown an entire application.

If your business relies on custom software, it's important that your code is high quality. As your software scales in complexity, bad code will [hold you back](http://gaslight.co/blog/breaking-open-gridlocked-rails-applications). Need help? [contact us](http://gaslight.co/contact)!