---
layout: post
post_author: Ryan Arthur
current_gaslighter: true
categories:
- Design
tags: []
permalink: "/blog/:slug"
post_title: How to Achieve The Content Scale Effect
publish_date: 2014-04-15T17:16:00.000+00:00
feature_post_image: ''
post_images: []
slug: how-to-acheive-the-content-scale-effect

---
The content scale effect has become a fairly popular design pattern across the web and mobile apps recently. This is the effect where the main content section of the UI is pushed, pulled, scaled, or rotated in some way to reveal additional information. In most cases it is used show and hide some sort of navigation or menu. Today I’d like to explore this effect in a slightly different context – image overlays. 


[View Demo](http://gaslight.github.io/posts/overlay_demo)

It is important to note that we will be using CSS3 transforms and transitions to achieve these effects so older browsers might not render them correctly. 

In this example we’ll scale our image back while fading in the overlay and give the text inside a slight upward motion while fading it in. First, lets start with out basic thumbnail component structure:

````css

<ul>
  <li class="thumbnail">
    <div class="thumbnail__overlay">
      <h3 class="title"> </h3>
      <p class="caption"> </p>
      <a class="button"> Read more <span class="button__arrow"> &#8594 </span>
    </div>
    <div class="thumbnail__image">
      <img src="...">
    </div>
  </li>
  <li class="thumbnail">
    ...
  </li>
  <li class="thumbnail">
    ...
  </li>
</ul>

````


Next we can add initial styles:

````css

.thumbnail {
  position: relative;
  overflow: hidden;
}

.thumbnail__image {
  transform-origin: bottom center;
  transition: transform 0.5s;
}

.thumbnail__overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  padding: 25px;
  background-color: rgba(87,192,232,0.85);
  opacity: 0;
  transform: translateY(100%);
  transition: transform 0.5s, opacity 0.5s;
}

.thumbnail__overlay .title,
.thumbnail__overlay .caption,
.thumbnail__overlay .button {
  opacity: 0;
  transform: translateY(20px);
  transition: transform 0.5s, opacity 0.5s;
  transition-delay: 0.3s;
}

````


And then layer on our transition styles:

````css

.thumbnail:hover .thumbnail__image {
  transform: scale(0.9);
}

.thumbnail:hover .thumbnail__overlay {
  transform: translateY(0%);
}

.thumbnail:hover .thumbnail__overlay .title,
.thumbnail:hover .thumbnail__overlay .caption,
.thumbnail:hover .thumbnail__overlay .button {
  opacity: 1;
  transform: translateY(0%);
}

````

Please check the demo for some more examples. I hope find this little experiment enjoyable and inspiring!