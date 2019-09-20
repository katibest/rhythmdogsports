---
layout: post
post_author: Chris Nelson
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Framework Independent Web Components
publish_date: 2019-01-21 13:27:00 +0000
feature_post_image: ''
post_images: []
slug: framework-independent-web-components

---
# Framework Independent Web Components

 The momentum behind Web Components is growing. Firefox has recently shipped full support for key Web Component standards. Microsoft just announced it was moving to rewrite Edge using the Chrome rendering engine. With Chrome having had support for Web Components for some time already, we can now clearly see a near future where all the major browsers support Web Components natively. And with good polyfills available for browsers that aren't quite there today, there's no reason not to start building with Web Components.

One of the main reasons for choosing Web Components is to avoid your application being coupled to the current javascript framework du jour. If you've lived with framework churn for a few years, this seems pretty compelling indeed. However, the Web Components specs don't attempt to standardize all of the features of a modern javascript web framework. This means that the ergonomic experience of building an app with plain vanilla Web Components can feel lacking. As a result, there are a lot of good libraries out there to improve the experience. This is great, but it can easily mean your components become coupled to said library or framework in the sense that they don't work without it. The question I'd like to consider in this article: How can we have a great developer experience without coupling our components to a library or framework? I'd like to suggest some patterns to follow that can help.

## 3 Simple Rules For Components without Coupling

### Pass data into components via attributes (or properties)

This isn't a new idea. Ember used to call this "data down, actions up". React calls this idea "presentational components". In the case of Web Components, passing simple data in via attributes works just fine. If you have more complex data, you can either pass in JSON or set a property of the component instance.

### Components re-render when data changes

Components need to re-render their DOM (be it shadow or otherwise) when they have new data. This can be done by observing attribute changes via component lifecycle hooks, or listening to property changes. This is also a place where libraries can help us without leading to coupling.

### Emit Custom Events when things happen

Custom Events give us a mechanism to notify the rest of the application when something interesting happens without coupling our component to a framework or library. If we were to choose another mechanism, redux actions for example, our component is now coupled to redux. If we want to rip out redux from our application or reuse our component in a non-redux application, we have work to do. With Custom Events, we are better leveraging the web platform.

Custom Events are a somewhat recent standard, but quite well supported. They are what the name implies: any DOM element can emit a CustomEvent with a specific name and a `detail` property that is essentially an arbitrary payload of data.

## What does this actually look like?

Let's explore this in the context of a web component application I've been working on recently: a weight and balance app for my flying club. When you fly an airplane, it's very important that two values are within certain limits specified by the manufacturer: Total Weight and Center of Gravity (C. G.). To calculate these values, you need to input the weight and location of everything you are going to put on the airplane. For our example, we'll build a simplified version: an app that lists known locations on the airplane (called stations in aviation vernacular) and allows the user to enter the weight for each. A resulting total weight is displayed.

Let's break this down into components that follow our guidelines. We'll start with the simplest, the `wb-total-weight` component. It takes weight passed in as an attribute and displays it, re-rendering any time the attribute changes.

```javascript
class WbTotalWeight extends HTMLElement {

  get weight() {
    return this.getAttribute("weight");
  }

  static get observedAttributes() {
    return ['weight'];
  }

  attributeChangedCallback() {
    this.render();
  }

  connectedCallback() {
    this.render();
  }
  
  render() {
    this.innerHTML = `
      <div>Total Weight: ${this.weight || ""}</div>
    `;
  }

}
```

By implementing the `observedAttributes` and `attributeChangedCallback` we ensure that this component redisplays any time the `weight` attribute changes.

Next, let's look at our `wb-station` component. This component dispatches a `CustomEvent` named `weightChange` any time the input field is edited.

```javascript
class WbStation extends HTMLElement {

  get name() {
    return this.getAttribute("name");
  }

  connectedCallback() {
    this.innerHTML = `
      <label>${this.name}<input type="number" /></label>
    `;
    this.querySelector("input").addEventListener("input", (event) => {
      this.dispatchEvent(new CustomEvent("weightChange", {
        bubbles: true,
        detail: {
          station: this.name,
          weight: Number(event.target.value)
        }
      }));
    })
  }
}
```

By following these rules, our components can remain quite simple and testable. Obviously though, something needs to listen to events dispatched from components and pass new data into components. I couldn't find anything existing that did quite what I wanted, so I wrote a very small bit of code that does the following:

* Listens to Custom Events dispatched from web components
* Applies reducer functions to calculate new state
* Passes data to interested web components after state changes

The resulting very small (< 50 lines) npm I've called [`wc-state-reducers`](https://www.npmjs.com/package/wc-state-reducers). The main function it exposes is `createStore`, which takes 3 required arguments: `element`, `reducers` and `subscribers`. `element` is the top level element to listen for Custom Events on: if there is only one store for the application, using `document` is appropriate. Both of the other two arguments are objects: `reducers` is an object (really a map) whose keys match the names of CustomEvents dispatched from components, and whose values are reducer functions. Each reducer function takes arguments for the current state and the event payload, and returns a new state. The `subscribers` argument is an object whose keys are css selectors and values are subscriber functions that get passed the new state and matched element after state changes occur.

In the context of our example app, here's how it looks:

```javascript
import { createStore } from 'wc-state-reducers';

const weightChange = (state, { station, weight }) => {
  const stations = Object.assign(state.stations || {}, {
    [station]: { station, weight}
  });
  const totalWeight = Object.keys(stations).reduce(((total, stationName) => { 
    return total + stations[stationName].weight;
  }), 0);
  return Object.assign(state, { stations, totalWeight });
};

const subscribers = {
  "wb-total-weight": ({ totalWeight }, element) => {
    element.setAttribute("weight", totalWeight);
  }
}

window.store = createStore(document, { weightChange }, subscribers);
```

In this case, we have a single reducer and subscriber. Our reducer populates a `stations` object on state with whichever station's weight has changed. Then it calculates the total weight by adding up the weight for all of the stations and adds it to the new state returned. The single subscriber function sets the `weight` attribute of the `wb-total-weight` element which causes it to re-render. The net result is that total weight is recalculated and displayed every time the value of the input for a station is changed.

## Hey wait, aren't you coupled to `wc-state-reducers`

This is definitely a fair question to ask! Here is why I would argue that our components are not coupled: *there are no references to `wc-state-reducers` in either component's code*. This may not seem like a big deal at first, but it means that I can take either component and drop into another application that uses some other means of state management. It also means I can remove `wc-state-reducers` and not have to touch any of my component code.

## Conclusion

Hopefully this gives you some ideas about how to build Web Components that don't couple you to a specific framework. In this article I've deliberately steered clear of using libraries in my web components. It's worth mentioning that there are several libraries that would let you implement my approach with less code and still avoid the coupling concerns I mention. Here are a couple that seem the most promising to me so far:

* [LitElement](https://lit-element.polymer-project.org/)
* [SkateJS](https://skatejs.netlify.com/)