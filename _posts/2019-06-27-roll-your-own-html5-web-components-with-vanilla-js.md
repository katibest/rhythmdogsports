---
layout: post
post_author: Chris Nelson
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Roll Your Own HTML5 Web Components with Vanilla JS
publish_date: 2014-02-20T15:39:00.000+00:00
feature_post_image: ''
post_images: []
slug: roll-your-own-html5-web-components-with-vanilla-js

---
I did a talk at CodeMash I entitled "The past, present, and future of web components". This article is about the future. The [W3C web components specs](http://www.w3.org/TR/components-intro/) are the bright and shiny future of web development. 

The code example I'll be presenting is so (currently) impractical that you can't even use it with any current production browser: you'll need to go get [Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html). And even then it still won't work! You'll need to specifically enable one of the specs we'll be using. Once you've opened Canary, visit about:flags. Search that page and find HTML Imports and enable that and restart. I'll wait and you can come back and finish reading...

All set? Good, let's dig in. If you've looked at the [W3C web components specs](http://www.w3.org/TR/components-intro/), you've seen that it's actually a meta-specification, meaning it's comprised of several other specifications. Each of these subspecs are fascinating and worth digging into, but I'm going to present the smallest example I could come up with to build something interesting, and show how all of these specs fit together. Then I'll drill into to each spec and talk about where it fits into the puzzle.

Also, I'll be using the excellent [vanilla.js framework](http://vanilla-js.com/) to help me build this example. It's highly functional and extremely light weight. You should check it out if you haven't yet!

Ok, so by now you've gotten the Vanilla.js joke. In all seriousness, not to knock excellent libraries like jQuery, etc, but for this article I thought it would be illuminating to see how how we can build rich web components with just the built-in APIs that will be coming to the browser.

## The Code

Here's our [example](http://gaslight.github.io/codemash2014/). If you view this in Chrome Canary, you should see a lovely name badge displaying Hello My Name is Bob. I've totally borrowed (think stolen) most of the pieces for this example from an excellent series [here](http://www.html5rocks.com/en/tutorials/webcomponents/shadowdom/). The only thing I've done is bring them all together into a concise example. So concise, in fact, that you'll hardly believe it:

````html
<html>
    <head>
        <link rel="import" href="name_badge.html"></link>
    </head>
    <body>
        <name-badge>Bob</name-badge>
    </body>
</html>
````

Wut? There's no javascript, no css, nothing! We see our custom element tag, `name-badge` but there's nothing in the document defining anything about what this element should do or look like.

This is what web components are all about. We can build arbitrarily complex user interface implements with associated styling and logic. We declaratively add them to 
our application via a custom element tag, `<name-badge>` in our case. The cooperating set of specs under the web components umbrella make all this possible.

## The HTML Imports Spec

The first spec we'll look at is [HTML imports](https://dvcs.w3.org/hg/webcomponents/raw-file/tip/spec/imports/index.html). HTML imports allow us to package our component neatly for inclusion in sites that want to use it. Notice the `link rel="import"` in the head section. This causes a browser which supports this spec to load the document from the given href. It doesn't cause any rendering to happen, mind you, it is basically fetching a chunk of DOM for use by the importing document. The imported document can contain, markup, css, script tags (which are executed) and basically anything an html document can contain. The difference is in the lack of rendering, and how scripts within the document execute, which we'll talk about shortly.

Not coincidentally, HTML imports make an excellent vehicle for packaging custom elements, which is exactly what we are doing. To see the gears turning, let's crack open the document we've imported:

````
<html>
  <template id="nameTagTemplate">
    <style>
    .outer {
      border: 2px solid brown;
      border-radius: 1em;
      background: red;
      font-size: 20pt;
      width: 12em;
      height: 7em;
      text-align: center;
    }
    .boilerplate {
      color: white;
      font-family: sans-serif;
      padding: 0.5em;
    }
    .name {
      color: black;
      background: white;
      font-family: "Marker Felt", cursive;
      font-size: 45pt;
      padding-top: 0.2em;
    }
    </style>
    <div class="outer">
      <div class="boilerplate">
        Hi! My name is
      </div>
      <div class="name">
        <content></content>
      </div>
    </div>
  </template>
  <script>
    var importDoc = document.currentScript.ownerDocument;
    var nameBadgePrototype = Object.create(HTMLElement.prototype);
    nameBadgePrototype.createdCallback = function() {
      var shadow = this.createShadowRoot();
      var template = importDoc.querySelector('#nameTagTemplate');
      shadow.appendChild(template.content.cloneNode(true));
    };
    document.registerElement("name-badge", {
      prototype: nameBadgePrototype
    });
  </script>
</html>
````

A little more to look at, huh? Don't panic, we'll walk through it a little at a time. 

## The HTML Templates Spec

The first thing we come across is a shiny new `template` element given to us by way of the [HTML Templates spec](https://dvcs.w3.org/hg/webcomponents/raw-file/tip/spec/templates/index.html). It's super keen. With it, we can define inert chunks of markup we can later copy, fill, and add to the DOM. As a web developer, it's highly likely you've used templates before on the client or server. What's nice is that templates are a first class citizen now, no need for ugly hacks like putting markup in script tags.

Inside of our template element we see a style block, and the markup that will render our name badge. The only thing unfamiliar is that `<content>` tag. The content tag is a new element that lets us access the markup inside our custom element `name-badge`, so we can choose where to display it. In this case, we're saying "Put all the content inside our custom element here." (hence the name content). If we wanted to be more specific and pick out bits, we could use a select attribute on our content tag.

After our template, we see a script block. This is what sets everything in motion. On the first line, we're grabbing ahold of our template so we can instantiate it, but the API we need to use is slightly different. This is something new in HTML imports. When a script block of an imported document is executed, `document` refers to the importing document, not the imported document. In our case though, we want to get the template in the imported document (ourself). We do that by using `document.currentScript.ownerDocument`.

## The Custom Elements Spec

Next, it's time to talk about [custom elements](https://dvcs.w3.org/hg/webcomponents/raw-file/tip/spec/custom/index.html), the spec that let's define our `<name-badge>` element. It's actually a pretty simple API: we call `document.registerElement` and pass in two arguments. The first argument is the name of the element -- per the spec, this must have a dash to differentiate custom from built in elements. The second argument is a hash options that let you specify the prototype of our new custom element. The prototype specifies what kind of object our browser will create for our custom element when it parses and builds the DOM. This lets us give our name-badge element an implementation. A custom element's prototype has to inherit prototypically from HTMLElement.prototype, which is the default if no prototype property is specified. 

## The Shadow DOM Spec

The custom element spec provides a number of lifecycle callbacks we can define on our custom element prototype. In our case, we want to use the `createdCallback` to do our rendering. Inside our callback, we find the last piece of the spec puzzle: the spooky, spooky, [Shadow DOM](https://dvcs.w3.org/hg/webcomponents/raw-file/tip/spec/shadow/index.html). It's got the coolest name of all the specs, and is also the most complex. Fortunately, we don't need to dive in too deep to talk about what's going on here. The key thing to understand about the Shadow DOM is that it provides DOM encapsulation. As in, things inside a Shadow DOM are not impacted or even visible to the surrounding DOM. 

It's also interesting that (in Chrome at least) you're already using the Shadow DOM without realizing it! Even wonder how browser vendors add new elements when a new spec (oh, say, HTML5) comes out? It turns out the way Chrome does it is with HTML and CSS. You can peek under the covers and see how this works. First, crack open your web inspector and under Setting (the little gear) click Show Shadow DOM. Next, go visit this handy [HTML5 example page](http://www.rbyers.net/chrome-builtin-shadowdom.html). Do an inspect element on any of those fancy new widgets to see a `#document-fragment` hidden inside. Expand that, and you see the Shadow DOM, markup the browser is secretly adding to implement the new HTML5 elements. Pretty neat, eh? And now that same magic is ours to wield!

The encapsulation Shadow DOM provides makes a lot of sense in the context of CSS. For our name-badge element, we want to have complete control over the appearance. We don't want to have the surrounding page bork up our name badge just because it happened to use the same css class we did. Shadow DOM is what helps us there. Notice the css inside our template element. If you were to style a name class in the surrounding document, it would have no impact at all on our name badge. Try it and see.

Creating a Shadow DOM is actually pretty simple: you call `createShadowRoot` on any element. It's important to note that a shadow will hide (or overshadow) any children you have in an element by default. However, you can have your original content appear in your Shadow DOM. This is exactly what our `<content>` element is doing. It's actually part of the Shadow DOM spec.

Lastly, but not leastly, we need to put something in our shiny new Shadow DOM. Doing this is simple too, we call       `appendChild` on our shadow root, passing in the cloned content of our template. The template element's content attribute (not the same as the content element!) is how you access the content of a template.

## The future is coming sooner than you think

Phew! That was a lot of explaining for not very much code. These specs are really going to make it easy and fun to build amazing things. It's worth learning them now, even though all the browsers don't support it yet. If you find yourself frustrated by this, it's also worth checking out [Polymer](http://www.polymer-project.org/). Polymer is a project consisting of polyfills for all of these specs that support modern browsers. It may or may not be something you can use in real projects today depending on what versions of browsers you need to support.

The other exciting things about these specs is that the major frameworks for client side development, such as Ember and Angular, are solidly on board with supporting them. In fact, a lot of these specs were driven in part by the needs of these frameworks.

It's going to be an awesome future for web development. See you all there!