---
layout: post
post_author: Alex Heflin
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'TIL: How to Correctly Use a Jasmine Spy'
publish_date: 2017-07-14 20:02:00 +0000
feature_post_image: ''
post_images: []
slug: til-how-to-correctly-use-a-jasmine-spy

---
At Gaslight we are all about continuously growing and learning, so we're starting a "Today I Learned Series"! Stay tuned for more tidbits!

I often find myself stumped when trying to use spies in a JavaScript test... Until Today.

TIL: How to correctly use a Jasmine Spy:

```
Foo = require('foo')

  describe 'Foo'
    describe 'bar'
      it 'does a thing'
        spyOn(Foo, 'bar')
        foo = new Foo()

        expect(foo.bar).toHaveBeenCalled()
```

Run into `no method error`? Try `spyOn(Foo.prototype, 'bar')`.

- Jasmine is a popular JavaScript testing framework
- Spies are a type of test double that allow you to stub functions and track the function call, and it's arguments

For more information on Jasmine Spies, visit their [documentation.](https://jasmine.github.io/2.0/introduction.html)