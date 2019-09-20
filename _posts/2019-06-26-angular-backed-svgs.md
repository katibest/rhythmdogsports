---
post_author: Kevin Rockwood
categories:
- Design
tags: []
post_title: Angular backed SVGs
publish_date: 2013-07-31T14:18:00.000+00:00
feature_post_image: ''
post_images: []
layout: post
current_gaslighter: false
slug: angular-backed-svgs
permalink: "/blog/:slug"

---
[Scalable Vector Graphics](http://en.wikipedia.org/wiki/Scalable_Vector_Graphics) are super cool. They're small compared to raster graphics and they're dynamic.

We recently created an application that needed a set of line graphs like this:

![](http://gaslight.github.io/posts/assets/images/svg_graph.png)

My immediate thought was to use a charting library like [NVD3.js](http://nvd3.org/) or [Raphael.js](http://raphaeljs.com/). We settled on NVD3.js, but the library started getting in our way.

## SVGs are just HTML!

SVG is an XML specification that all modern browsers support. You can drop it into any HTML page and it just works. And just like HTML, you can interact with its elements using CSS. Think of it as HTML with its own set of elements and attributes for drawing things.

We work in HTML every day and we've gotten pretty good at it. When using NVD3 (or D3, Raphael, etc.), I found myself doing things like `.attr("stroke-width", "2px")` and `.attr("fill", "#fff")`. Then it dawned on me... wait a sec, I'm writing HTML with JavaScript! Haven't we decided that that's a bad idea? On top of that, the HTML coming out had all these nasty in-line styles and useless wrapper elements. What am I doing?

So I started over and just wrote the SVG the way I wanted it:

```html
<svg xmlns="http://www.w3.org/2000/svg" class="chart__content">
  <path class="chart__line" d="M20,50 L100,80 L200,40 L280,30"></path>
  <circle class="chart__circle" cx="20" cy="50" r="5"></circle>
  <circle class="chart__circle" cx="100" cy="80" r="5"></circle>
  <circle class="chart__circle" cx="200" cy="40" r="5"></circle>
  <circle class="chart__circle" cx="280" cy="30" r="5"></circle>
</svg>
```

Simple, and I can make it look nice with just a couple CSS rules:

```css
.chart__content {
  background: #b6e356;
}

.chart__line {
  fill: none;
  stroke: #fff;
  stroke-width: 2px;
}

.chart__circle {
  fill: #fff;
}
```

The key is that we're separating markup from presentation. No more defining colors in JavaScript. All our styling happens in CSS and our markup is clear and clean.

Here's the [complete JSBin](http://jsbin.com/imocuf/latest/edit)

## But what about dynamic data?

You'll notice that we're duplicating our point coordinates to draw both the line and the circles, and we have that nasty path string in our html. Thats where Angular comes in. By binding a couple attributes, we can get a dynamic chart that's backed by real data. Although this example is in Angular, it translates well to other JavaScript frameworks like [Ember](http://emberjs.com/) or [Backbone](http://backbonejs.org/).

We add bindings to our HTML:

```html
<svg xmlns="http://www.w3.org/2000/svg" ng-controller="GpaController" class="chart__content">
  <path class="chart__line" ng-attr-d="{% raw %}{{linePath()}}{% endraw %}"></path>
  <circle ng-repeat="p in points" ng-attr-cx="{{p.x}}" ng-attr-cy="{{p.y}}" r="5" class="chart__circle"></circle>
</svg>
```

For an explanation of why we need to use ng-attr, [checkout this](https://github.com/angular/angular.js/pull/2061).

And in our controller:

```javascript
window.App = angular.module('App', []);

App.controller('GpaController', function($scope){
  var points = [
    {x: 20, y: 50},
    {x: 100, y: 80},
    {x: 200, y: 40},
    {x: 280, y: 30}
  ];

  $scope.points = points;

  $scope.linePath = function(){
    var pathParts = [], currentPoint, i;

    for (i = 0; i < points.length; i++) {
      currentPoint = points[i];
      pathParts.push(currentPoint.x + "," + currentPoint.y);
    }

    return "M" + pathParts.join(" L");
  };
});
```
Here's the [complete JSBin](http://jsbin.com/ujasub/latest/edit)

The cool part is that our chart updates whenever the `points` data changes.

This is a fairly static example. In our actual implementation, we calculated the chart size at runtime and dynamically calculated x and y values from an array of model data. We also distilled everything down into directives so we could just say something like `<line-chart data="{{points}}">`.

If you need really fancy charts with animations then maybe a full-fledged SVG library is the right choice. But if you just need a simple, custom chart then I'd recommend starting with hand-rolled SVGs. Going that route first would have saved me a big headache.

## Update

I've added [some fun hacks](http://jsbin.com/ihatad/latest/edit) just to show whats possible.

Special thanks to [postmodest](http://www.reddit.com/user/postmodest) and [goosefraba190](http://www.reddit.com/user/goosefraba190) for their experiments
