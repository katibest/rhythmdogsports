---
post_author: Mitch Lloyd
categories:
- Development
tags: []
post_title: Client-Side IDs with Ember Data
publish_date: 2013-06-26T15:35:00.000+00:00
feature_post_image: ''
post_images: []
layout: post
current_gaslighter: false
slug: client-side-ids-with-ember-data
permalink: "/blog/:slug"

---
## Why Client-Generated IDs?

In most client-side frameworks (e.g. Backbone, Angular, Ember) models created
in the web browser get temporary IDs (client IDs) and get their permanent
ID from the server after a successful save.

For client-heavy applications, generating IDs on the client can simplify
interaction with the server.  Consider the case where the user creates a post
with several attachments.  You'll need to make sure that the post is saved and
has its ID resolved before attempting to save the child models in order to
preserve this relationship. Take a look at [this Ember
issue](https://github.com/emberjs/data/pull/724) and you'll start to
appreciate the complexity of this use case.

It can be liberating to know the ID of a model before it is saved. While
uploading an attachment, you could associate it with other models or send
someone a link to where the attachment will be. Users could work offline
and save records to the database later without worrying about how to resolve
and update IDs.

Because we're using
[UUIDs](http://en.wikipedia.org/wiki/Universally_unique_identifier), you could
hypothetically find any record in your application given only an ID. I've not
seen this used for anything practical yet, but there could be interesting
possibilities if we kept a global store of IDs and their associated objects on
the client.

If you find yourself generating secret IDs for resources (think
account activation URLs) you may save yourself some code by using unique IDs.
Security-wise, [it really can't
hurt](http://www.theinquirer.net/inquirer/news/2079431/citibank-hacked-altering-urls).

## A Brief History of (My Experience With) Client-Side IDs

I first encountered client-generated IDs when working with
[Spine](http://spinejs.com/). Looking back at older
versions you can
see that [Spine included a UUID
generator](https://github.com/spine/spine/blob/v0.0.9/src/spine.coffee#L433).
Every model was [was given a unique
ID upon creation](https://github.com/spine/spine/blob/v0.0.9/src/spine.coffee#L311).

Spine Author, Alex Maccaw, [was critical of Backbone's
temporary client IDs](http://old.alexmaccaw.com//posts/async_ui):

> ... a solution that Backbone uses is generating an internal cid (or
> client id). You can use this cid temporarily before the server responds
> with the real identifier... I'm not such a fan of that solution, so I've
> taken a different tack with Spine.

However, in one commit that says it all (["add CIDs (inspired by
Backbone)"](https://github.com/spine/spine/commit/3bec17a6b1ac4560e42a123b312db33d7ae68678))
Alex brought Spine inline with Backbone's
server-generated ID strategy.

Despite this new direction, [Spine remained
flexible](https://github.com/spine/spine/pull/229) and is a great platform for
client-generated IDs. Backbone isn't as friendly for client-generated IDs,
mainly because [it uses the existence of a permanent ID to determine whether a
record has been
persisted](https://github.com/documentcloud/backbone/blob/1.0.0/backbone.js#L550).
But, as Zed Shaw would say, Programming Motherf*er.

```coffeescript
class App.Model extends Backbone.Model
  initialize: ->
    unless @id
      @set('id', utils.guid()) unless @id
      @_new = true
      @once 'sync', => @_new = false

  isNew: ->
    @_new
```

## Client-Side IDs with Ember Data and PostgreSQL

Ember Data is [rarin' to go with client-generated
IDs](https://github.com/emberjs/data/blob/9d6173c48829439aae71bfe6d8a5bf9fffc1dd1b/packages/ember-data/lib/system/adapter.js#L607-L630).
Override the dedicated method in the RESTAdapter and you're golden.

```coffeescript
DS.RESTAdapter.reopen
  generateIdForRecord: ->
    'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace /[xy]/g, (c) ->
      r = Math.random()*16|0
      v = if c == 'x' then r else (r&0x3|0x8)
      v.toString(16)
```

That code and [the inclusion of a specification for client-generated IDs on
jsonapi.org](http://jsonapi.org/format/#id-based-json-api) gives me confidence that
Ember will continue to be a good fit for this approach.

Even though we're talking about client-generated IDs, there will be times when
the server needs to generate IDs of its own. If you're using Mongo, you're in
luck -- it already uses unique IDs.  Here's how you can whip up some matching
unique IDs in PostgreSQL and Rails 4 with a couple of migrations.

```ruby
# Enable UUID generation in PostgreSQL
class SetupDatabase < ActiveRecord::Migration
  def change
    enable_extension "uuid-ossp"
  end
end
```

```ruby
class CreateDocuments < ActiveRecord::Migration
  def change
    # Use {id: :uuid} to use UUIDs as the primary key.
    create_table :areas, id: :uuid do |t|
      t.string :name
      # t.belongs_to :category will use an integer. You'll need to setup your
      # associations like this.
      t.uuid :category_id
    end
  end
end
```

## Tradeoffs

I haven't hit any major pain points with client-generated IDs yet but
here are some quick considerations before you dive in:

* `Model.first` and `Model.last` won't behave the way you expect in Rails, since
  the IDs may not be sequential.
* There are some rumblings on the Internet about the impact of unique IDs relation join performance, but I couldn't find anything conclusive.
* URLs are uglier.

So give client-generated IDs a try if you've got a good use case. It can
go a long way to making a user experience less dependent on a round trip
to the server.

Want to learn the latest on Ember Data? [Check our our updated Introduction to Ember.js class](https://teamgaslight.com/training/courses/14-early-access-new-introduction-to-ember-js) online to learn about Ember from its creators.
