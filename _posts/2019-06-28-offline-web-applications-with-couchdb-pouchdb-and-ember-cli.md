---
layout: post
post_author: Chris Moore
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Offline Web Applications with CouchDB, PouchDB and Ember CLI
publish_date: 2015-12-10 13:00:00 +0000
feature_post_image: ''
post_images: []
slug: offline-web-applications-with-couchdb-pouchdb-and-ember-cli

---
A few years ago, we worked with a client in the agricultural industry. We were building an application to allow scouts to take measurements out in the field. Literally.

![](http://40.media.tumblr.com/c5096b1c303c7f687e3754fede2e249d/tumblr_moozs5TsHZ1qlk81uo2_1280.jpg)

It was a difficult problem because we had to support offline use. We wound up creating a pretty sophisticated system that worked well, but it took almost a year of development effort from the team. There were many other features, but offline use consumed a large portion of our effort.

In the end, we created a native iOS app designed to run on iPads. The iOS app was built to be offline friendly, but it didn’t start out that way.

Initially, we thought we could continue to develop in the typical web fashion: build the API and the client application in parallel. After some flailing, we realized that we were actually building two applications that happened to synchronize data between them.

## Offline first

There has been a lot of activity in the “offline first” space recently. A team in Germany has been working with health care teams [on the ground in Africa fighting Ebola](http://www.ehealthafrica.org). They used JavaScript and the web to power offline web applications. These apps enable health care workers to track and update patient care records in the field without cellular service.

But your app doesn’t have to be mission critical to benefit from the offline first approach. Consider the user who is working in your application on one of the few remaining planes without Wifi. Or taking the train underground with no cell service. Or working inside a huge, metal warehouse that basically acts as a Faraday cage. There are still areas where working offline can be beneficial.

## The test

Another client has approached us with an idea for an application that will need to support the online/offline use case. For this example, we’ll use the idea of someone tasked with inspecting something. Think about an inspector heading out in to the world to take readings. Some of the areas where the work will happen are more rural and might not have cellular service.

## Setup

As an avid supporter of [all things Ember](https://teamgaslight.com/training/courses/14-introduction-to-ember-js), this will be my starting point. I’ve upgraded my [ember-cli](http://ember-cli.com/) to the latest release (1.13.13 at the time of this writing). Next, I’ll create a new, empty application.

```
ember new offline
cd offline
```

Next, we’ll install [CouchDB](https://couchdb.apache.org). If you aren’t familiar with Couch, you should be! Couch is designed from the ground up to make replication easy and has very good conflict resolution support with built in versioning.

UPDATE: There's a much, much easier way. Thanks @janl!

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/cdmwebs">@cdmwebs</a> …installer from <a href="https://t.co/mTz9ELDlSU">https://t.co/mTz9ELDlSU</a> — 1. unzip 2. double click 3. there is no step 3 :)</p>&mdash; Jan Lehnardt (@janl) <a href="https://twitter.com/janl/status/675040196249919488">December 10, 2015</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

`brew install couchdb`

This step will take a while unless you’ve already installed Erlang. After Couch installation finishes, we need to enable CORS so that our application can connect. Make sure your Couch instance is running (just type `couchdb`). There's a handy package called [add-cors-to-couchdb](https://github.com/pouchdb/add-cors-to-couchdb) that will do this for us:

```
npm install -g add-cors-to-couchdb
add-cors-to-couchdb
```

Ok, now let’s get back to the Ember CLI project. Our app will need a local data store as well. We could just use local storage, but I’m going to use a neat little micro instance of CouchDB that runs in the browser called [PouchDB](http://pouchdb.com). Since Couch supports sync already, Pouch can act as a “mini-Couch” and will handle subsets of our data very well.

Let’s install [Ember Pouch](https://github.com/nolanlawson/ember-pouch) into our project.

`ember install ember-pouch`

If you start your server, you should see a message that says “Welcome to Ember” when you browse to http://localhost:4200. If you do, let’s move on to our test application.

### The app

First, we need to configure our Ember application adapter. ember-pouch makes this pretty easy. Here’s my application adapter:

```javascript
// app/adapters/application.js

import PouchDB from 'pouchdb';
import { Adapter } from 'ember-pouch';

PouchDB.debug.enable('*');

var remote = new PouchDB('http://localhost:5984/offline');
var db = new PouchDB('local_pouch');

db.sync(remote, {
   live: true,   // do a live, ongoing sync
   retry: true   // retry if the conection is lost
});

export default Adapter.extend({
  db: db
});
```

I’ve enabled debugging, which will clog up our Console with messages, but that’s okay for now. It’s kind of fun to watch.

Now let’s create our model: List the inspections on the index page and add a form to create a new inspection.

```javascript
// app/models/inspection.js

import DS from 'ember-data';

export default DS.Model.extend({
  title       : DS.attr('string'),
  isCompleted : DS.attr('boolean'),
  rev         : DS.attr('string')
});
```

Our model is pretty basic. That’s okay. Do note the `rev` attribute, though. That’s required by Pouch and Couch. It’ll handle revisions for us.

Add an `/inspections` route and load the inspections:

```javascript
// app/router.js

Router.map(function() {
  this.route('inspections');
});
```

```javascript
// app/routes/inspections.js

import Ember from 'ember';

export default Ember.Route.extend({
  model() {
  return this.store.findAll('inspection');
  }
});
```

Create a template to show the list and add a new inspection (I’m using Bootstrap markup):

```handlebars
{% raw %}
<!-- app/templates/inspections.hbs -->
<div class="row">
   <div class="col-md-3">
     <h2>New Inspection</h2>
     <form {{action "createInspection" on="submit"}}>
       <div class="form-group">
         <label>Title</label>
         {{input value=title class="form-control"}}
       </div>
       <div class="checkbox">
         <label>
           {{input type="checkbox" name="isCompleted" checked=isCompleted}}
           Complete?
         </label>
       </div>
       <input type="submit" value="Add Inspection" class="btn btn-primary">
     </form>
   </div>

   <div class="col-md-9">
     <h2>Inspections</h2>
     <table class="table">
       <thead>
         <tr>
           <th>Revision</th>
           <th>Title</th>
           <th>Complete?</th>
           <th></th>
         </tr>
       </thead>

       <tbody>
         {{#each model as |inspection|}}
           <tr>
             <td>{{inspection.rev}}</td>
             <td>{{inspection.title}}</td>
             <td>
               {{#if inspection.isCompleted}}
                 &#x2713;
               {{else}}
                 &#x2717;
               {{/if}}
             </td>
             <td {{action "deleteInspection" inspection on="click"}}>
               x
             </td>
           </tr>
         {{/each}}
       </tbody>
     </table>
   </div>
 </div>
{% endraw %}
```

And a controller to handle the `createInspection` and `deleteInspection` actions:

```javascript
// app/controllers/inspections.js

import Ember from 'ember';

export default Ember.Controller.extend({
  actions: {
    createInspection() {
      var inspection = this.store.createRecord("inspection", {
        title: this.get('title'),
        isCompleted: this.get('isCompleted')
      });
      inspection.save();

      this.set('title', '');
      this.set('isCompleted', false);
    },

    deleteInspection(inspection) {
      inspection.deleteRecord();
      inspection.save();
    }
  }
});
```

That’s it. You’ve just built your first offline ready web application. Seriously! Let’s give it a try.

First, start CouchDB.

`couchdb`

You’ll see a bit of output in the console then a notice that Couch is ready and it’s now time to relax:

```
Apache CouchDB 1.6.1 (LogLevel=info) is starting.
Apache CouchDB has started. Time to relax.
[info] [<0.32.0>] Apache CouchDB has started on http://127.0.0.1:5984/
```

Once Couch is up and running, start your Ember application (if it isn’t already).

`ember serve`

Now, browse to http://localhost:4200/inspections and fill out the form. You should see a new inspection created in the table.

![](http://g.recordit.co/ncUhIxt1zB.gif)

Also, if you browse to Futon (CouchDB’s admin tool), you’ll see a new record there as well.

![](https://gaslight-blog.s3.amazonaws.com/offline-web-applications-with-couchdb-pouchdb-and-ember-cli/Screen_Shot_2015-12-08_at_12.08.47_AM.png)

## Let’s go offline!

Now the exciting part. The moment we’ve all been waiting for! Let’s stop our Couch server and pretend we’re offline. Hop over to the terminal window running `couchdb` and give it a `CTRL-C`. Jump back over to your Ember app running in the browser and refresh. It’s still up and running! Go ahead and add another inspection or two. Refresh the page. Whoa! You’re creating new inspections offline! Also, look in the console and watch PouchDB try to connect to the server. You’ll see it try to reconnect every few seconds.

Let’s see if we can synchronize our data. Start `couchdb` again and pretend we just picked up a cell signal. Hop back over to the Ember app and refresh (this will trigger a sync). Now look in Futon and you’ll see your new records. Isn’t that amazing?

Oh, by the way, deleting works, too. Go ahead, give it a shot. You know you want to! I’ll wait.

## What are you going to build?

That was just a simple example, but I hope you’ve thought of some interesting uses for offline web applications. The use cases for this model are all over the place. A few years ago, it took months to create something like this. You and I, because of the genius of others, are now able to do it in a few minutes.

You can view the source code for this article [here](https://github.com/cdmwebs/inspector/).
