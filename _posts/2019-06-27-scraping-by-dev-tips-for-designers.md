---
post_author: Kristin Lasita
categories:
- Development
- Design
tags: []
post_title: 'Scraping By: Dev Tips for Designers'
publish_date: 2013-07-29T14:27:00.000+00:00
feature_post_image: "/uploads/dev-for-des.png"
post_images: []
layout: post
current_gaslighter: false
slug: scraping-by-dev-tips-for-designers
permalink: "/blog/:slug"

---
You are a designer working in web today. Because you make the things pretty, you are used to chucking your designs over the developer wall and leaving it there.

Wrong.

Any designer worth their salt working in interfaces has to have some basic understanding of how to run a dev project. I don't care how pretty your Dribbble post is, if the feature you added will take 2 weeks, it becomes unnecessary.

On the flipside, you don't need to be one of those des/dev demi-gods to work with developers.  Just learn to love Github, throw around a few terms, and embrace the terminal (really).

# Learn GitHub... The Important Bits

[Github](http://github.com/) is one of the best tools to learn when working with developers. Long story short, Github allows a group of people to work on code and store it all together. If you want to collaborate on anything code-based, you should sign-up. 95% of the time it's wonderful and everything works out. There are plenty of tutorials out there if you are completely new and don't have a developer around to help you through it. Plus, Github has a wonderful interface to it, inviting you to explore and learn all the things you can do. The octo-cat is pretty cool. The part that I love as a designer about Github, is the huge number of open-source projects. There are tons of smart people out there putting together these wonderful tools that are free for your use. Really.

Cool, Github sounds great! How do you do I get the code off my computer and onto the site? The terminal! Terrifying? Let's try out something easier. I use [Sourcetree](http://www.sourcetreeapp.com/) to interact with Github. It allows you to easily get the changes from your coworkers, see the edits you made, and then put those changes back on Github. Again, this program is much more robust, but I use it for little else besides pushing and pulling from Github.

# Learn the Lingo

Pushing and pulling? I'll be the first to admit that dev terminology is confusing. Like anything, these take time to learn, but you can focus your effort on some of the most commonly used terms. Below are some of the terms I hear thrown around the most, with my super interpretation. You can Google it if you want more specifics, because Internet.

**Repo** Short for a repository, which is the place where all the code lives on Github.

**Clone the Repo** Basically what it says. You are making a local copy of the repository on your computer. This will be called the "master" branch.

**Branch** If you're being awesome, you'll create an offshoot of master where you put your work. This is a good idea because if you completely break everything, you can delete the branch and the master is completely fine.

**Pull/Commit/Push** These go hand in hand. You _pull_ code from the repo onto your computer. Make your changes and put these into a _commit_ with a nice descriptor like "Make Everything Blue!". You _push_ your changed code back into repo on Github, so everyone can now see all the wonderful things you made blue.

**Pull Request** When you're happy with the work you did on your branch, you can send out a pull request to the owner of the repository. If they are happy with all the wonderful things you made blue, they will merge this into master.

**Merge Conflict** Uhoh, I f$%#ed up. Call over a developer to help you untangle your twisted mess.

# The Terminal is Your Friend...Really

My only experience with the terminal before working with developers was using it to make my computer say things. Not rocket science. Now I use it to do productive things without killing my computer. Novel concept. There are an infinite number of things you can do in Terminal, I use 3 commands that cover my butt 95% of the time. They are:

* cd
* bundle
* rails s

Seriously, very rare I use anything else.

**cd** Short for "change directory", this allows me to navigate into the folder where the code lives.

**bundle** If I get any sort of error screen at all when I run the project, I tell the project to bundle. Since I work in Ruby projects, this command goes out and fetches all the gems (tricky bits of code) that the project may use.

**rails s** This is how I get a rails project running so I can see it in the browser. You can also cheat and use [Pow](http://pow.cx/) running on [Anvil](http://anvilformac.com/), although this gets developers cranky.

# Conclusion

Like I said, this is just a basic working guide. There are plenty of other things to pick up and learn. Really the best way to learn is to work with developers. See what tools they use and how to use them. Ask what the best solution is to something you are working on. Hopefully this short guide helps ease the transition between designers and developers.
