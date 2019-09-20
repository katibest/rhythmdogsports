---
layout: post
post_author: Bill Barnett
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: How to Start Developing Mobile Apps With AngularJS and Ionic Framework
publish_date: 2016-02-09 15:00:00 +0000
feature_post_image: ''
post_images: []
slug: how-to-start-developing-mobile-apps-with-angularjs-and-ionic-framework

---
We've been developing mobile applications at Gaslight for years. As is to be
expected, our methods have matured in pace with advancements within the domain.
We've been fans of [Cordova](https://cordova.apache.org/) for some time. Bus
Detective's [mobile app](https://github.com/bus-detective/mobile-client) is one
example but it's essentially just a Cordova wrapper of
[BusDetective.com](http://busdetective.com/).

Recently we were approached by a client who is seeking to [add off-line data
collection](https://teamgaslight.com/blog/offline-web-applications-with-couchdb-pouchdb-and-ember-cli) to their existing Web platform. The challenge we faced was adding a
detailed and very specific workflow to their platform that would needlessly
complicate the Web interface. From previous work with this client, we decided it
would be beneficial to decompose the existing platform into a number of
[microservices](http://martinfowler.com/articles/microservices.html) if we were
given the opportunity. That opportunity just arrived.

### Ionic Framework
The process we're modeling for the client requires users to perform off-line
data collection in a factory environment. Mobile was the clear choice. After an
extensive search for tools that would enable us to leverage existing knowledge
and tools while targeting Android and iOS platforms we chose
[Ionic](http://ionicframework.com/), a framework for creating mobile apps using
HTML, CSS ([Sass](http://sass-lang.com/)) and JavaScript
([Angular](http://angularjs.org/)).

Ionic's "[Getting Started](http://ionicframework.com/getting-started/)" docs
are thorough but beware that Node 5 is not yet supported. Having
[nvm](https://github.com/creationix/nvm) installed is helpful if you're
doing bleeding edge Node development. The TL;DR is run these commands to create
a *blank* Ionic app.

```sh
npm install -g cordova ionic
ionic start myApp blank
```

This results in a bare bones app with an intuitive project file structure.

```sh
myApp
├── bower.json
├── config.xml
├── gulpfile.js
├── hooks
├── ionic.project
├── package.json
├── platforms
├── plugins
├── scss
│   └── ionic.app.scss
└── www
    ├── css
    ├── img
    ├── index.html
    ├── js
    │   ├── app.js
    │   ├── controllers.js
    │   └── services.js
    ├── lib
    └── templates
        ├── chat-detail.html
        ├── tab-account.html
        ├── tab-chats.html
        ├── tab-dash.html
        └── tabs.html
```

The tree above omits the underlying framework code but even [novice Angular
developers](https://teamgaslight.com/training/courses/16-mastering-angularjs) should be able to make sense of the file structure and begin hacking
in features. However in doing so, the first problem to arise would be that the
`app.js`, `controllers.js` and `services.js` files would become unwieldy pretty
fast. Immediately, we felt the need to organize our code into concise Angular
modules.

And what about testing? Certainly you'll want decent test coverage for your
app. (I know your client will!) How will you organize that code?

Finally, `ionic serve` serves the application from the `www` directory, which
made the idea of keeping that directory as clean and tidy as possible very
appealing. We found [GulpJS](https://teamgaslight.com/blog/small-sips-of-gulp-dot-js-4-steps-to-reduce-complexity) helped us achieve that goal,
ensuring none of the support code made it into the app and gave us flexibility
in managing development environments.

### ES6 Modules and Babel
It would probably be easy enough to use [RequireJS](http://requirejs.org/) to
help with code organization, but we've already been using
[ES6 modules](https://babeljs.io/docs/learn-es2015/#modules) effectively on
other projects. Without finicky browsers to worry about, it seemed like a
slam dunk to continue that practice for this project.

We've been using [Babel](https://babeljs.io/) for ES6 to JavaScript compilation
on the aforementioned projects and found it simple to integrate with the
`gulpfile.js` task (below) for compiling our
[Jasmine](http://jasmine.github.io/) specs, which are then run via
[Karma](https://karma-runner.github.io/0.13/index.html).

```javascript
var babel = require("gulp-babel");
var browserify = require('browserify');
var vinylSourceStream = require('vinyl-source-stream');
var vinylBuffer = require('vinyl-buffer');
...
gulp.task("specs", function () {
  var sources = browserify({
    entries: './specs/specs.js',
    debug: true
  }).transform(babelify.configure({ presets: ['es2015'] }));

  return sources.bundle()
    .pipe(vinylSourceStream('specs.js'))
    .pipe(vinylBuffer())
    .pipe(gulp.dest('./specs/dist'));
});
```

ES6 modules helped massively with code organization. Here's the simplified
`app.js` file in ES6:

```javascript
import { default as controllersModuleName } from './controllers/app.controllers';
import { default as servicesModuleName } from './services/app.services';
import { default as routesModuleName } from './routes';
import { default as constantsModuleName } from './constants';

var moduleName = 'app';

var app = angular.module(moduleName, ['ionic', constantsModuleName, controllersModuleName, routesModuleName, servicesModuleName])
.run(function($ionicPlatform) {
  $ionicPlatform.ready(function() {

    // Hide the accessory bar by default (remove this to show the accessory
    // bar above the keyboard for form inputs)
    if(window.cordova && window.cordova.plugins.Keyboard) {
      cordova.plugins.Keyboard.hideKeyboardAccessoryBar(true);
    }
    if(window.StatusBar) {
      // org.apache.cordova.statusbar required
      StatusBar.styleDefault();
    }
  });
});

export default moduleName;
```

Note that the controllers, services, routes and even environment constants are
imported from appropriate locations within the `app` directory. We decided
upon a `app/<module type>/app.<module type>.js` naming convention for the base
files, which import all modules of that type. For example, here's our
`app/controllers/app.controllers.js` file:

```javascript
import HomeController from './home.controller';
import WorkOrdersController from './work_orders.controller';
import PerformInspectionController from './perform_inspection.controller';
import StyleguideController from './styleguide.controller';

var moduleName = 'app.controllers';

angular.module(moduleName, [])
  .controller('HomeController', HomeController)
  .controller('WorkOrdersController', WorkOrdersController)
  .controller('PerformInspectionController', PerformInspectionController)
  .controller('StyleGuideController', StyleguideController)

export default moduleName;
```

We followed this pattern for all the app code and features specs but more on
the subject of testing in a future post. The end result looks a bit tidier
and provides a nice contextual separation between the app code, test specs and
built/compiled code.

```sh
myApp
├── app
│   ├── controllers
│   ├── scss
│   ├── services
│   ├── templates
│   ├── app.js
│   ├── constants.js
│   └── routes.js
├── config             # Environment configs, e.g., development, staging, etc.
├── hooks
├── node_modules
├── platforms          # Native code in here still once the app has been built.
├── plugins
├── specs
├── www                # Only compiled and minified assets in here!
│   ├── css
│   ├── img
│   ├── js
│   └── lib
├── config.xml
├── gulpfile.js
├── ionic.project
├── karma-conf.js
├── mock-api.js        # ExpressJS mock HTTP service for testing.
├── package.json
├── protractor-conf.js
└── run-features.sh    # Script that runs the feature specs.
```

After some initial tinkering, we're now pleased with how we have our code
organized. We've fallen into a pattern of knowing where to place things and
how to include them in the Angular app.

### Gotchas!
Here are a couple of nagging issues we encountered. Nothing major.

1. After pulling a commit in which `package.json` was changed, remembering to
run:
```sh
npm install
```

1. Managing environments is troublesome and we haven't ironed everything out
just yet, but roughly if we want to target any environment other than
staging we have to run:
```sh
gulp --env production
ionic build android
```

### Next Steps
In future posts, we will detail testing Ionic-based apps, including feature
testing, as well as managing environments from development to production, plus
solutions we've devised to any other sticking points we encounter along the way.
If you have experience developing mobile applications with Angular or Ionic we'd
appreciate any feedback you'd care to offer in the comments below.
