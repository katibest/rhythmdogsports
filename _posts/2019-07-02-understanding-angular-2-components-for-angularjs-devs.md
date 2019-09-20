---
layout: post
post_author: Joel Turnbull
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Understanding Angular 2 Components for AngularJS Devs
publish_date: 2016-07-08 14:00:00 +0000
feature_post_image: ''
post_images: []
slug: understanding-angular-2-components-for-angularjs-devs

---

What is it like to build an Angular 2 app, and how is the experience different from AngularJS?

[Angular 2](https://teamgaslight.com/blog/a-sneak-peek-at-angular-2) is component based. Components combine concepts that we are already familiar with from AngularJS. The Angular 2 Component combines the AngularJS Directive, Controller, and Scope. My article will attempt to make you more comfortable with components by comparing them to what you already know from AngularJS.

Here are some tutorials that I worked through to prepare this. I will periodically link to code examples from them:
[Quickstart & Tour of Heroes](https://angular.io/docs/ts/latest/)

TypeScript is recommended for Angular 2. You will see the `.ts` file extension. I will point out TypeScript syntax where needed.

## Bootstrapping the Top Level Component

Here is an example of an Angular 2 component, `app.component.ts`, that I've copied below from the Angular 2 Tutorial.

```
import { Component, OnInit } from '@angular/core';

import { Hero } from './hero';
import { HeroDetailComponent } from './hero-detail.component';
import { HeroService } from './hero.service';

@Component({
  selector: 'my-app',
  template: '
    <h1>{{title}}</h1>
    <h2>My Heroes</h2>
    <ul class="heroes">
      <li *ngFor="let hero of heroes"
        [class.selected]="hero === selectedHero"
        (click)="onSelect(hero)">
        <span class="badge">{{hero.id}}</span> {{hero.name}}
      </li>
    </ul>
    <my-hero-detail [hero]="selectedHero"></my-hero-detail>
  ',
  styles: ['
    .heroes {
      margin: 0 0 2em 0;
      list-style-type: none;
      padding: 0;
      width: 15em;
    }
  '],
  directives: [HeroDetailComponent],
  providers: [HeroService]
})

export class AppComponent implements OnInit {
  title = 'Tour of Heroes';
  heroes: Hero[];
  selectedHero: Hero;

  constructor(private heroService: HeroService) { }

  getHeroes() {
    this.heroService.getHeroes().then(heroes => this.heroes = heroes);
  }

  ngOnInit() {
    this.getHeroes();
  }

  onSelect(hero: Hero) { this.selectedHero = hero; }
}
```

Before we delve into the details, let me describe what is happening here at a high level. First we import any referenced classes using TypeScript modules at the top. Next we configure the component using the @Component decorator. The decorator is a feature of TypeScript. Finally, we define the component's properties and behavior then export it.

[Angular 2 apps](http://info.teamgaslight.com/build-an-angular-2-and-phoenix-app?hs_preview=vzIAyVrG-4199557700) will have one main, top-level Component. Using the ES6 module system (supported by TypeScript), we can kick-off the application by importing the AppComponent and bootstrapping it as the main component like this: 

```
// main.ts
import {bootstrap} from 'angular2/platform/browser';
import {AppComponent} from './app.component';

bootstrap(AppComponent);
```

Loading `main.js` in a `<script>` tag will start the app on the page through it's top-level component.

## The @Component Decorator

TypeScript decorators, like `@Component`, are functions that modify the class defined below. The Angular 2 `@Component` decorator is a configuration much like the AngularJS Directive. Compare the following AngularJS Directive to the Angular 2 `@Component` decorator above.

```
directive('myHero', function() {
  return {
    restrict: 'E',
    scope: {
      customerInfo: '=info'
    },
    templateUrl: 'my-hero.html',
    controller: 'HeroController',
    controllerAs: 'heroCtrl'
  };
});
```

Components and directives define the HTML markup to use. Components also define the styles attribute where directives do not. In components, styles are encapsulated. They do not "leak" to elements in the outer HTML or to other Components. This is enabled by some improvements in HTML itself.

Styles and HTML markup need not be defined inline. @Component provides the `templateUrl` and `styleUrl` properties all alternative to `template` and `style`.

```
@Component({
  selector: 'my-dashboard',
  templateUrl: 'app/dashboard.component.html',
  styleUrls: ['app/dashboard.component.css']
})
```

# Components Continued

Angular 2 components make some decisions for you that directives didn't. For example, Angular 2 components:

- Always isolate scope (instead of sharing a parent scope)
- Always restrict 'E' (load into custom elements in the DOM)
- Always bind to a controller (as opposed to $scope)

AngularJS applications that adhere to the [John Papa Style Guide](https://github.com/johnpapa/angular-styleguide) already use these conventions. I would strongly recommend referencing the guide while developing your AngularJS apps.

## Component Class Definition

Angular 2 component class definitions do the same job as our AngularJS controllers. At the bottom of `app.component.ts` we `export` the `AppComponent` component class definition. It's where we define and initialize the state of our component and define the component's behaviour as functions that events on the page bind to.

```
import {Component, OnInit} from 'angular2/core';
import {Hero} from './hero';
import {HeroDetailComponent} from './hero-detail.component';
import {HeroService} from './hero.service';

@Component({
  selector: 'my-app',
  template:'
    <h1>{{title}}</h1>
    <h2>My Heroes</h2>
    <ul class="heroes">
      <li *ngFor="#hero of heroes"
        [class.selected]="hero === selectedHero"
        (click)="onSelect(hero)">
        <span class="badge">{{hero.id}}</span> {{hero.name}}
      </li>
    </ul>
    <my-hero-detail [hero]="selectedHero"></my-hero-detail>
  ',
  directives: [HeroDetailComponent],
  providers: [HeroService]
})

export class AppComponent implements OnInit {
  title = 'Tour of Heroes';
  heroes: Hero[];
  selectedHero: Hero;

  constructor(private _heroService: HeroService) { }

  getHeroes() {
    this._heroService.getHeroes().then(heroes => this.heroes = heroes);
  }

  ngOnInit() {
    this.getHeroes();
  }

  onSelect(hero: Hero) { this.selectedHero = hero; }
}
```

Angular 2 provides the `ngOnInit` lifecycle hook for you to step in and initialize the component. We initialize data through the `ngOnInit` hook instead of through the constructor to limit side effects in our tests. Notice we only use the constructor function to wire things up.

## Component Interactions

`AppComponent` will render itself into `<my-app>`, by setting its own element selector to `my-app` in the `@Component` decorator. It will also render a HeroDetailComponent into `<my-hero-detail>`. It does a few things to make this happen:

- Imports the `HeroDetailComponent` class
- Registers the `HeroDetailComponent` in its `directives` metadata array
- Provides the `<my-hero-detail>` element in its template markup
- Sets the values of `HeroDetailComponent`'s input properties through `<my-hero-detail>`'s element attributes

Here is the implementation of the `HeroDetailComponent`:

```
import { Component, Input } from '@angular/core';
import { Hero } from './hero';

@Component({
  selector: 'my-hero-detail',
  template: '
    <div *ngIf="hero">
      <h2>{{hero.name}} details!</h2>
      <div>
        <label>id: </label>{{hero.id}}
      </div>
      <div>
        <label>name: </label>
        <input [(ngModel)]="hero.name" placeholder="name"/>
      </div>
    </div>
  '
})
export class HeroDetailComponent {
  @Input() hero: Hero;
}
```

When the app is bootstrapped, `AppComponent` is directed through its `selector` property to seek out a `<my-app>` element on the page to render itself into. Its sub-component, `HeroDetailComponent`, will similarly seek out a `<my-hero-detail>` element through its `selector` property.

The `<my-hero-detail>` element is provided in the markup of the `HeroDetail`'s parent component, `AppComponent`, and the `<my-app>` element is provided in `index.html`. Users of [angular ui-router](https://github.com/angular-ui/ui-router) will recognize this pattern of rendering nested views into `<ui-view>` elements.

When the user selects a hero, `AppComponent` is responsible for passing that information along to the `HeroDetailComponent`.

`<my-hero-detail [hero]="selectedHero"></my-hero-detail>`

In the `<my-hero-detail>` element, the `AppComponent` binds the hero property of the HeroDetailComponent to the current value of its `selectedHero` property. We also declare hero as an `@Input` property `HeroDetailComponent`'s class definition (see `HeroDetailComponent` code above). The interesting side-effect of this is that data-binding on `selectedHero`, and the `<div *ngIf="hero">` in `HeroDetailComponent`'s template is the trigger that renders the `my-hero-detail` component on the page.

A property that is bound in this way, through a sub-component's element, must be declared as an `@Input()` in the sub-component. Failing to do so will force Angular 2 to reject the binding and throw an error. In Angular 2, we explicitly declare bindable properties as `@Input()`, so that we can protect internal properties from being munged from the outside.

The bracket syntax on `[hero]` represents a property data binding. This is a one-way binding of `currentHero` from the component to an element. Angular 2 adds new template syntax to represent different bindings in the markup.

One-way binding means the value of `currentHero` cannot be updated by `hero-detail`. A two-way binding to `currentHero` would be represented like `[(hero)] = "currentHero"`. This example doesn't touch all the types of data-binding in Angular 2. You can read more about event bindings, etc, [here](https://angular.io/docs/ts/latest/guide/template-syntax.html#!#ngModel). 

## Dependency Injection

`AppComponent` depends on the `HeroService` in order to access the application's hero data. We register the `HeroService` as a dependency in the `@Component` decorator's `providers` property. In our providers array we define the services we are interested in, and they are made available in our component's constructor.

Notice that we do not re-declare the `HeroService` as a dependency of the `HeroDetail` component. Dependencies need not be re-declared in sub-components, and doing so could be detrimental. Redeclaring `HeroService` in this way would give `HeroDetail` a new instance of the `HeroService`. This defeats the purpose of the service, which is to share the same data amongst different components.

## Conclusion

I wrote this article to demonstrate some differences in the development experience between Angular 2 and AngularJS by looking at one specific new feature, Components. I hope components provide you with a new way of thinking about your applications, simultaneously making them more flexible, extendable, and maintainable. 

Angular 2 introduces many other improvements, both in the code and in areas like performance and mobile support. I invite you to explore and discover them for yourself. Happy Angularing!

