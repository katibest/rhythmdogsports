---
layout: post
post_author: Michael Guterl
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Wrapping jQuery with React
publish_date: 2016-02-16 14:30:00 +0000
feature_post_image: ''
post_images: []
slug: wrapping-jquery-with-react

---
Are you interested in migrating your existing jQuery project to React?

Maybe you're just interested in using a jQuery plugin in your project and there's no React component that handles your needs.

Using a lot jQuery plugins in your React project is not something that I would encourage, but in these situations wrapping a jQuery plugin with React might be your best option.

## An Approach

The first step when wrapping a jQuery plugin with React is to create a component to manage the jQuery plugin. This component will provide a React-centric view of the jQuery component. In this tutorial I'll show you how to: 

* Use [React Lifecycle Methods][lifecycle] to initialize and teardown the jQuery plugin
* Map props to plugin configuration options so you can configure the plugin for different use cases
* Truncate multi-line content with [jQuery.dotdotdot][dotdotdot]

[lifecycle]: https://facebook.github.io/react/docs/component-specs.html#lifecycle-methods "React Lifecycle Methods"

To make it easier to follow along, I'm going to provide a real world example.

## An Example

If you didn't know, truncating multi-line content is non-trivial on the web. Fortunately there's a jQuery plugin that can help us with this:

[jQuery.dotdotdot][dotdotdot], advanced cross-browser ellipsis for multiple line content.

Let's start by looking at an example using the jQuery plugin and we'll slowly convert it over to React. You can also [see the original jQuery implementation in action][jsbin-jquery]. I have consolidated this example to a single file so that it is easy for you to try on your own.

[dotdotdot]: http://dotdotdot.frebsite.nl/ "jQuery.dotdotdot"

## Original jQuery Example

See the code in action at [JSBin][jsbin-jquery]

[jsbin-jquery]: http://jsbin.com/jihurakazo/edit?html,output "jQuery.dotdotdot JSBin"

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://code.jquery.com/jquery-2.1.4.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jQuery.dotdotdot/1.7.4/jquery.dotdotdot.min.js" type="text/javascript"></script>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
  
  <script type="text/javascript">
    $(document).ready(function() {
      $('.dotdotdot').dotdotdot();
    })
  </script>
  <style type="text/css">
    .dotdotdot {
      height: 50px;
    }    
  </style>
</head>
<body>
  <p class='dotdotdot'>
    Cincinnati paul brown stadium freedom center historic architecture
    rivertown central christian moerlein fifty west 1788 union
    terminal river front slavery cyclones midpoint music festival.
    1788 fifty west humid john roebling otr.  Washington park city
    reds cincinnati coffee emporium humid main street hyde park.
    Freedom center hyde park zinzinnati over-the-rhine museum center
    immigrants city walnut hills washington park flying pig
    oktoberfest isaac m. wise temple cyclones city beat union terminal
    reds.
  </p>
  <p class='dotdotdot'>
    Cincinnati paul brown stadium freedom center historic architecture
    rivertown central christian moerlein fifty west 1788 union
    terminal river front slavery cyclones midpoint music festival.
    1788 fifty west humid john roebling otr.  Washington park city
    reds cincinnati coffee emporium humid main street hyde park.
    Freedom center hyde park zinzinnati over-the-rhine museum center
    immigrants city walnut hills washington park flying pig
    oktoberfest isaac m.  wise temple cyclones city beat union
    terminal reds.
  </p>
</body>
</html>
```

### 1. Create a component

In general I like to solve really small problems and iteratively add complexity. Let's start by getting jQuery, jQuery.dotdotdot, and React working together.

```javascript
const DotDotDot = React.createClass({
  componentDidMount: function() {
    // Every React component has a function that exposes the
    // underlying DOM node that it is wrapping. We can use pass that 
    // DOM node to jQuery and initialize the plugin.

    // You'll find that many jQuery plugins follow this same pattern 
    // and you'll be able to pass the component DOM node to jQuery 
    // and call the plugin function.
    $(ReactDOM.findDOMNode(this)).dotdotdot();
  },

  render: function() {
    return <div>
      {this.props.text}
    </div>;
  }
});
```

We can then use the component in our code:

```javascript
<DotDotDot text="Text that you want to truncate" /> 
```

### 2. Pass configuration options via props

In all but the most trivial cases you're going to need to pass configuration options to the jQuery plugin. Instead of hardcoding this into our component, we can pass the configuration in via component props.

Don't ask me why, but you're interested in using ☃ instead of ellipsis. We can setup the component to pass the prop down to the jQuery plugin.

```javascript
componentDidMount: function() {
  $(ReactDOM.findDOMNode(this)).dotdotdot({ 
    ellipsis: this.props.ellipsis 
  });
}
```

```javascript
<DotDotDot 
  ellipsis="☃"
  text="Text that you want to truncate" />
```

This works, but there are a few drawbacks: 

* You must specify ellipsis everywhere you use the component
* You must explicitly map props to configuration options

We can use [Object Rest Destructuring][] to extract only the options that we want to pass down.

[Object Rest Destructuring]: https://github.com/sebmarkbage/ecmascript-rest-spread/blob/master/Rest.md "Object Rest Destructuring"

```javascript
componentDidMount: function() {
  // We know that text is a prop that we don't want to pass to dotdotdot
  // because it's part of our API, not dotdotdot, so we extract that and 
  // pass everything else down.
  const { text, ...config } = this.props;
  $(this.getDOMNode()).dotdotdot(config);
}
```

Now any option that the plugin supports now (or in the future) is handled automatically.

```javascript
<DotDotDot
  callback={this.handleTruncation} 
  fallbackToLetter={false}
  text="Text that you want to truncate"
  watch={false} />
```

### 3. Clean up after yourself

Many jQuery plugins provide a mechanism for cleaning up after themselves when they're no longer needed. DotDotDot provides an event that we can trigger to tell the plugin to unbind its DOM events, remove CSS classes, etc. [React Lifecycle Methods][lifecycle] comes to the rescue again and provides a mechanism to hook into when the component is being unmounted.

```javascript
componentWillUnmount: function() {
  $(ReactDOM.findDOMNode(this)).trigger('destroy.dot');
}
```

### Conclusion

You can see the final version working with React [here](http://jsbin.com/pahixofefe/edit?html,js,output)

```javascript
const DotDotDot = React.createClass({
  componentDidMount: function() {
    const { text, className, ...options } = this.props;
    $(ReactDOM.findDOMNode(this)).dotdotdot(options);
  },
  
  componentWillUnmount: function() {
    $(ReactDOM.findDOMNode(this)).trigger('destroy.dot');
  },
  
  render: function() {
    return <div className={this.props.className}>
      {this.props.text}
    </div>;
  }
});
```

Wrapping jQuery plugins with React isn't always the best choice, but it is nice to know that it is an option. It is a viable option if you're migrating a legacy jQuery application to React or maybe you just can't find a React plugin that suits your needs.

This is not the first time that I have been interested in how jQuery and other JavaScript frameworks interact, 2 years ago I took a look at integrating [jQuery Knob with Angular](https://github.com/mguterl/angular-knob).

One thing that I did not cover in this article is integrating React into your existing codebase. We're big fans of Ruby on Rails and we've had great success integrating React and Rails via the most excellent [react-rails](https://github.com/reactjs/react-rails). I recommend looking for similar plugins for your platform if you are considering migrating your project to React.

