---
layout: post
post_author: Jim Anders
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Getting Started with Protractor and Page Objects for AngularJS E2E Testing
publish_date: 2014-04-23T13:00:00.000+00:00
feature_post_image: ''
post_images: []
slug: getting-started-with-protractor-and-page-objects-for-angularjs-e2e-testing

---
When it was announced that Protractor would be replacing the karma runner as *THE* end to end test framework for AngularJS, I'll admit, I was a bit sad. While I didn't have a deep love for the karma runner, it was crazy simple to setup and run, and my initial perusal of the Protractor docs was intimidating. After spending some time with Protractor though, I have come to enjoy using it in the same manner in which I would test a Rails application.

Going through the Protractor docs and getting started can still be a daunting task. My goal is to alleviate some of that by sharing my configuration and setup, as well as using the page object pattern for creating an API through which we'll interact with our web pages. We'll also be using the jasmine-given library to help clean up our specs.

## The Setup

The first thing we will need to do is make sure we have Protractor and jasmine-given available to our application.

```bash
$ npm install protractor jasmine-given --save-dev
```

Please note the `--save-dev`. This is so that node will add these as dev dependencies to `package.json`. It is also important to note that Protractor can be installed globally with the `-g` switch. Personally, I do not like to install application dependencies globally, and try to avoid them if at all possible.

Protractor has two dependencie of it's own, selenium server and a browser driver. There exists pages of documentation for running the selenium server stand alone and managing browser drivers, but Protractor gives us a script to manage both.

In your project directory, run the following command:

```bash
$ node_modules/protractor/bin/webdriver-manager update
```

This will install the selenium server jar file and the chrome driver executable locally to your project. Now to start the selenium server, simply run the following command in a separate terminal window.

```bash
$ node_modules/protractor/bin/webdriver-manager start
```

The last piece of our setup puzzle is the config file that we'll pass to Protractor when running our specs.

In spec-e2e.conf.js

    exports.config = {
      seleniumAddress: "http://127.0.0.1:4444/wd/hub",
      seleniumPort: null,
      seleniumArgs: [],
      specs: [
        '../spec-e2e/**/*spec.{js,coffee}'
      ],
      capabilities: {
        'browserName': 'chrome'
      },
      baseUrl: 'http://localhost:8000',
      jasmineNodeOpts: {
        onComplete: null,
        isVerbose: false,
        showColors: true,
        includeStackTrace: false
      }
    };

With that, we tell protractor where to find our selenium server, which browser(s) to test against, and any jasmine options we choose to pass along. You may reference the example [config](https://github.com/angular/protractor/blob/master/referenceConf.js) in the Protractor repo for additional config options.


## Our first test

For a simple example of using Protractor to test drive an application, we are going to use logging into a web application. In `spec/e2e/login_spec.js` we setup our test using jasmine-given and page objects.

    require("protractor/jasminewd");
    require('jasmine-given');
    
    var LoginPage = require("./pages/login_page");
    
    describe("app", function() {
      var page = new LoginPage();
      describe("visiting the login page", function() {
        Given(function() {
          page.visitPage();
        });
        describe("when a user logs in", function() {
          Given(function() {
            page.fillEmail("testy@example.com");
          });
          Given(function() {
            page.fillPassword();
          });
          When(function() {
            page.login();
          });
          Then(function() {
            page.getCurrentUser().then(function(text) {
    	   expect(text).toEqual("Randy Savage");
            });
          });
        });
      });
    });

First we require our protractor jasmine web driver, and also jasmine-given. Next we require our not yet existing login page object. Last, we walk through the steps of navigating to the login page, filling in and submitting the form. Last, we assert that our page knows about our `currentUser` and that it's text is equal to Randy Savage.

We can run this spec with the following command:

```
$ node_modules/protractor/bin/protractor path/to/spec-e2e.conf.js
```

The output from this run should throw an error that node cannot find the module `pages/login_page`, so let's go ahead and add that.

```
var LoginPage = (function () {
    function LoginPage() {
        this.emailField = element(By.input("user.email"));
        this.passwordField = element(By.input("user.password"));
        this.loginButton = element(By.id("log-in"));
        this.currentUser = element(By.binding("{{currentUser.name}}"));
    }

    LoginPage.prototype.visitPage = function () {
        browser.get("/");
    };

    LoginPage.prototype.fillEmail = function (email) {
        this.emailField.sendKeys(email);
    };

    LoginPage.prototype.fillPassword = function (password) {
        if (password == null) {
            password = "password";
        }
        this.passwordField.sendKeys(password);
    };

    LoginPage.prototype.login = function () {
        this.loginButton.click();
    };

    LoginPage.prototype.getCurrentUser = function () {
        return this.currentUser.getText();
    };

    return LoginPage;

})();

module.exports = LoginPage;
```

In the constructor function of our `LoginPage` class, we setup instances of the things on the page we wish to interact with using Protractor's locators. We use `By.input` to find an input with a binding to `ng-model="user.email"` and again for `ng-model="user.password"`. We find our submit button by id, and lastly we find the current users name by searching the page for the `currentUser.name` binding. We then setup the functions on our page object that we are calling from our spec, using the elements we setup in our constructor. It is important to note the implicit return of `this.currentUser.getText()`. This is because the `getText()` method returns a promise, which we return to and resolve in our spec. Finally we ensure that our page object is available to the spec by exporting it.

Now, when we run our spec again, we should get an actionable failure `Error: No element found using locator: by.model("user.email")` allowing us to implement our markup and logic to get our spec passing. With just a few simple steps, we were able to get a selenium server running and Protractor configured to use it. As well as creating a pleasant testing DSL by abstracting our page specific login into a reuseable page object.