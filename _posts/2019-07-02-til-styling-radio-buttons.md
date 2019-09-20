---
layout: post
post_author: Alex Heflin
current_gaslighter: true
categories:
- Design
tags: []
permalink: "/blog/:slug"
post_title: 'TIL: Styling Radio Buttons'
publish_date: 2017-09-22 19:07:00 +0000
feature_post_image: ''
post_images: []
slug: til-styling-radio-buttons

---
Styling radio inputs doesn’t have to be daunting! Save yourself some time and CSS heartache with these simple steps:

## Three Simple Tips to Styling Radio Buttons:

1. Always reset the appearance of your radio button. This gets rid of default styles from the browser, leaving you a blank canvas to work with.
2. Pseudo elements are your friends! They allow you to create elements directly tied to your radio button, and allow you to create custom colored or styled "inner circle".
3. Use CSS selector `:checked` on your input to style your radio button and pseudo elements when it's selected.


<p data-height="350" data-theme-id="0" data-slug-hash="EwPqJx" data-default-tab="css,result" data-user="heflinao" data-embed-version="2" data-pen-title="Radio Styles" class="codepen">See the Pen <a href="https://codepen.io/heflinao/pen/EwPqJx/">Radio Styles</a> by Alex Heflin (<a href="https://codepen.io/heflinao">@heflinao</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


```html
<fieldset>
  <input class="radio triangle" type="radio" id="triangle" name="shapes" checked>
  <label for="triangle">Triangle</label>
  <input class="radio circle" type="radio" id="circle" name="shapes">
  <label for="circle">Circle</label>
  <input class="radio square" type="radio" id="square" name="shapes">
  <label for="square">Square</label>
</fieldset>

<fieldset>
  <input class="radio cat" type="radio" id="cat" name="animals" checked>
  <label for="cat">Cat</label>
  <input class="radio dog" type="radio" id="dog" name="animals">
  <label for="dog">Dog</label>
</fieldset>
```


```CSS
/* You could choose to override all browser styles on all your inputs, but I prefer explicit classes */
.radio {
  /* secret sauce to styling radio buttons */
  appearance: none;
  -webkit-appearance: none;
  -mozilla-appearance: none;
  /* Aligning your label text and your radio */
  vertical-align: text-bottom;
}

/* Setting the checked styles */
.radio:checked {
  border-color: pink;
}

.triangle {
  border: 20px solid red;
  border-top: 15px solid transparent;
  border-left: 15px solid transparent;
  border-right: 15px solid transparent;
}

.triangle:checked {
  border-bottom-style: double;
  border-top: 15px solid transparent;
  border-left: 15px solid transparent;
  border-right: 15px solid transparent;
}

.circle {
  border: 10px solid red;
  border-radius: 100%;
}

/* radio outer circle */
.circle:checked {
  border-width: 5px;
}

/* radio inner negative circle */
.circle:checked:before {
  content: "";
  height: 10px;
  width: 10px;
  display: block;
}

.square {
  border: 10px solid red;
}

/* creating the illusion of inner and outer elements with double borders */
.square:checked {
  border-style: double;
}

/* you can even use images */

.cat {
  background: url('https://s3.amazonaws.com/gaslight-blog/til-styling-radio-buttons/photo.jpg');
  background-size: auto 50px;
  display: block;
  height: 50px;
  width: 80px;
  opacity: 0.5;
}

.cat:checked {
  opacity: 1;
}

.dog:after {
  content: "DOG";
}

.dog:checked:after {
  content: "DOG CHECKED";
}
```
