---
post_author: Chris Moore
categories:
- Design
tags: []
post_title: Block, Element, Modifier
publish_date: 2013-08-12T13:05:00.000+00:00
feature_post_image: ''
post_images: []
layout: post
current_gaslighter: false
slug: block-element-modifier
permalink: "/blog/:slug"

---
## Why?

CSS is the last frontier of web development. It's usually a cluttered,
unorganized mess of random styles crammed in a file. There's always some
attempt at keeping things organized, but the lack of structure
eventually catches up and now you're spending an hour digging through
the CSS in the inspector.

There are a few "pioneers" in this space, though. They're bringing some
much needed structure to the world of CSS. A couple of patterns have
emerged: SMACSS and BEM.

SMACSS attempts to stay as generic as possible, which is noble. I'm a
firm believer that generic styles are the most powerful. Create the
"look and feel" and implement the [style guide][kristin], then start
building.

Building web applications is a little different, though. We're really
creating systems of components that can be re-used throughout
the application. Ideally, anyway.

This is where BEM makes sense. As developers, we like object oriented
code. We like namespaces and objects. BEM is an attempt to replicate
that syntax with CSS.

## What is it?

Block, Element, Modifier.

**Blocks** are independent entities. Components, if you will. An example of
a block might be a search form.

**Elements** are, well, elements. They're the HTML elements that exist
within the block. In our search form example, they might be:

* the input field
* the button

**Modifiers** are "states". For example, when we're using the search form,
we might want to highlight it. We could add an `active` modifier to the
block to indicate that.

## How Does it Work?

It's actually pretty simple, once you understand the reasons. Let's keep
using the search form. Here's the markup, without classes.

```html
<form method="get" action="/search">
  <label>
    Search
    <input type="text" name="search" />
  </label>
  <input type="submit" value="Find Stuff!" />
</form>
```

Here's the markup using BEM:

```html
<form method="get" action="/search" class="search">
  <label class="search__label">
    Search
    <input type="text" name="search" class="search__field" />
  </label>
  <input type="submit" value="Find Stuff!" class="search__action"/>
</form>
```

Here's the CSS that we might use:

```css
.search {
  border: 1px solid #ddd;
  padding: 10px;
}

.search__label {
  font-weight: bold;
}

.search__field {
  width: 30%;
  margin-right: 10px;
}

.search__action {
  width: 10%;
}
```

Now, let's add that active state we talked about:

```css
.search--active {
  box-shadow: 0px 0px 12px rgba(50, 50, 50, 0.75);
}
```

Add it to the markup:

```html
<form method="get" action="/search" class="search search--active">
  <label class="search__label">
    Search
    <input type="text" name="search" class="search__field" />
  </label>
  <input type="submit" value="Find Stuff!" class="search__action"/>
</form>
```

And here it is:

<style type="text/css">
  .search {
    border: 1px solid #ddd;
    padding: 10px;
  }

  .search__label {
    font-weight: bold;
  }

  .search__field {
    width: 30%;
    margin-right: 10px;
  }

  .search__action {
    width: 20%;
  }

  .search--active {
    box-shadow: 0px 0px 12px rgba(50, 50, 50, 0.75);
  }
</style>

<form method="get" action="/search" class="search">
  <label class="search__label">
    Search
    <input type="text" name="search" class="search__field" />
  </label>
  <input type="submit" value="Find Stuff!" class="search__action"/>
</form>

And here it is active:

<form method="get" action="/search" class="search search--active">
  <label class="search__label">
    Search
    <input type="text" name="search" class="search__field" />
  </label>
  <input type="submit" value="Find Stuff!" class="search__action"/>
</form>

## In the Wild

Want to learn more about BEM? Check out the [BEM information site][BEM]. It's
chock full of graphics, illustrations and code samples.

There's a pretty awesome Sass framework called [inuit.css][inuit] <del>that's authored
by the creator of BEM, too</del>. We're currently investigating this for our
own use.

Also, check out the [introduction to BEM][intro] on CSSWizardy, too.

[kristin]: http://gaslight.co/blog/style-guides-or-how-i-learned-to-stop-worrying-and-love-the-grid
[BEM]: http://bem.info
[inuit]: http://inuitcss.com/
[intro]: http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/

UPDATE: I fixed my incorrect HTML syntax. Thanks [admiralteal](http://www.reddit.com/r/css/comments/1k7emq/is_bem_the_perfect_structure_for_css/cbm54z0)!

Also, Harry isn't the author of BEM. [Yandex is](http://bem.info/method/history/).
