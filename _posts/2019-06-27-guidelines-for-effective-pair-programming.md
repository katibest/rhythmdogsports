---
layout: post
post_author: Michael Guterl
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Guidelines for Effective Pair Programming
publish_date: 2014-06-03T14:00:00.000+00:00
feature_post_image: ''
post_images: []
slug: guidelines-for-effective-pair-programming

---
> Pair programming is an agile software development technique in which two
> programmers work together at one workstation. One, the driver, writes code
> while the other, the observer, pointer or navigator, reviews each line of
> code as it is typed in. The two programmers switch roles
> frequently.[1](http://en.wikipedia.org/wiki/Pair_programming)

Pair programming is one of many techniques we use at Gaslight for producing
high quality software. Teams are not required to pair all the time, but it can
be effective in certain situations. Below are some guidelines that have emerged
from our experience with pair programming.

## Shared Workstation / Physical Environment

If you opt to share a physical workspace while pair programming there are extra
considerations to take into account. Dedicated pairing stations that are not
owned by a single developer can help reduce some of these problems.

### Avoiding Poor Ergonomics

When pairing, it is not always important to be sitting at the same desk. Many
of us have found that sharing a single desk with another person leads to poor
posture.  People of different sizes have unique desk and chair height needs, it
can be difficult to accommodate both individuals. Sharing a single display
causes your neck to be turned for long periods of time. 

Posture and ergonomics is important and it should be taken into consideration
any time you're at a workstation. Using [ScreenHero](http://screenhero.com/) or
the [native Mac OS screen sharing](http://support.apple.com/kb/PH14148) while
sitting diagonal from one another has worked well for us. Sitting diagonal from
one another also allows you to see one another without twisting sideways.

### Personal Hygiene

![hygeine](http://gaslight.github.io/posts/assets/images/pairing.png)

* Shower 
* Brush your teeth 
* Wear deodorant

Don't share a workspace if you're sick, you shouldn't even be at the office.
You'll most likely end up getting your pair sick and decrease the productivity
of the team.

### Clean Workspace

* Free of clutter 
* Sanitized (alcohol wipes are your friend)

Dedicated pair programming stations reduce some of these issues, but they're
not always practical.

## Virtual Environment

The physical environment is not the only important factor when pairing, the
virtual environment is also important. These tips are also tips for general
productivity and can be applicable when working without a pair.

### No extra windows

Close any windows not related to the problem that you're working on currently.
Extra text editor windows, email, unrelated browser tabs or windows, and
anything else should not be open. Extra windows cause distraction and make
using alt-tab to switch through applications more difficult.

### Common Editor

Both pairs need to be effective with the text editors that is being used during
the pairing session. If you both use Vim during your normal day, feel free to
use it. Consider adding a custom vimrc and sourcing it to give your pair
keybindings they're comfortable with. If one of you uses Vim and the other
Emacs, you shouldn't argue about which editor to use, pick something neutral.
We like [Atom](http://atom.io).

### Organized Windows

Come up with a sane window layout so that you can see what's important and have
fast feedback loops. [Moom](http://manytricks.com/moom/) gives you the ability
to save and restore window layouts. It also has a nice grid for resizing and
moving windows into position.

### Close Email, Close Chat

Pair programming is not the time to catch up on email or chat with your
significant other about what's for dinner tonight. You're wasting your time and
your pair's time if you do this.

### Turn Off Notifications

If you're using Mac OS you should turn off notifications (option-click
notification center).

### Offline Notes

Keep a notepad on the desk so you don't have to interrupt your pair when you
notice something that you need to do later.

### Turn Off Your Phone

Like email and instant messaging, your phone is an unnecessary distraction and
shouldn't be in your hand while pairing. You're supposed to be engaged in the
process and if your phone is in hand, you won't be.

### Develop Rhythm

Bounce back and forth frequently. It is not effective to have one person
driving the whole time. The person who isn't coding is likely to end up bored
and you won't realize the full benefits of pair programming.

### Avoid Conversation With Others

We are working in a mostly open office space and if you're talking with someone
else while your pair is working, you're not helping the cause.

### Know Your Tools

This is important whether you're pairing or working by yourself. It's important
to understand the tools you're working with.

### Avoid Boilerplate

Often times there are tasks which are rather mundane that are not challenging.
Know when you should pair up and when it may be better to work on your own.
Remember that when you're pairing you're using two people's time.