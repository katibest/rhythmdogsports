---
post_author: Chris Nelson
categories:
- Development
tags: []
post_title: Backbone.js Views Done the Right Way
publish_date: 2012-06-06T23:56:00.000+00:00
layout: post
current_gaslighter: true
feature_post_image: ''
post_images: []
slug: backbone-dot-js-views-done-the-right-way
permalink: "/blog/:slug"

---
As soon as you build an interesting application in backbone, one of the
challenges you are likely to encounter is wanting to have composite views, or
views that are contained within a larger view.  I've solved this problem
several ways in different projects and I thought it would be fun to walk
through the progression and how I've arrived at what I currently see as a
preferred solution.

Let's start by talking about what might seem to be the most obvious solution
and why it doesn't actually work.  We'll use as an example a view that
displays a collection of people.  Let's assume we'd like to have a view for
the table and within it, views for each row.  You might end up with a
TableView like so:

```coffeescript
class Example.Views.TableView extends Backbone.View

  render: ->
    @$el.html JST["table_view_template"] @

  tableRow: (person) ->
    tableRowView = new Example.Views.TableRowView(model: person)
    tableRowView.render()
    tableRowView.$el.html()
```

With the template for it being:

```haml
%table.table-bordered
  %thead
    %tr
      %th First Name
      %th Last Name
  %tbody
    - for person in @collection.models
      = @tableRow(person)
```

I currently using haml_coffee as my favorite templating language.  If you dig
Haml, it's worth checking out.

Notice the tableRow method on TableView.  Because we pass the view itself into
the template function, tableRow is available to us in the template.  In it we
create a TableRow, render it, and then return it's html.  Here's what
TableRowView and it's template looks like

```coffeescript
class Example.Views.TableRowView extends Backbone.View

  render: ->
    @$el.html JST["table_row_view_template"] @
```

```haml
%tr
  %td= @model.get("first_name")
  %td= @model.get("last_name")
```

And initially it all appears to work well.  We see our table with a row for
each person.  Great!  But now let's try adding an event handler that pops up
an alert when we click a table cell.

```coffeescript
class Example.Views.TableRowView extends Backbone.View

  events:
    "click td": "clicked"

  clicked: ->
    alert "Way to go, you clicked a cell!"

  render: ->
    @$el.html JST["table_row_view_template"] @
```

And what happens?  Precisely nothing.  What's going on?  The problem is that
the TableRowView's element never actually gets added to the DOM.  We create a
TableRowView in the tableRow method of TableView, render into it's element,
and then pull out the row view elements html and shove it into the rendered
output of TableView.  We grabbed the html, but TableRowView's element never
actually made it into the DOM.  That means event binding, jQuery plugins, and
all kinds of stuff just won't work.  Not good.

Let's try another approach:

```coffeescript
class Example.Views.TableView extends Backbone.View

  render: ->
    @$el.html JST["table_view_template"] @
    for person in @collection.models
      tableRowView = new Example.Views.TableRowView(model: person, el: @$("#row_#{person.id}"))
      tableRowView.render()

  tableRow: (person) ->
    "<tr id='row_#{person.id}'></tr>"
```

Here we've changed the tableRow method to not create the TableRowView at all,
but instead to create a row element with an id.  We then add a second loop
thru the collection at the end of render and create our TableRowView, passing
in it's element which we find using the id we gave it and then tell it to
render.  Because the row view's element is in the DOM at the time we create
and render it, everything works.  When I first starting building complexish
apps in backbone, this is how I generally did composite views.

But it's pretty clunky.  We have a second loop thru the collection for one
thing, and it just doesn't feel very clean.  The code relating to adding rows
is now in two different places, and the parent seems to have pretty intimate
knowledge of how the child view works.

We can do better.  Backbone gives a powerful tool for decoupling in events,
and we can use them here to make our code cleaner.  Let's try another crack at
TableView:

```coffeescript
class Example.Views.TableView extends Backbone.View

  render: ->
    @$el.html JST["table_view_template"] @
    @trigger "rendered"

  tableRow: (person) ->
    new Example.Views.TableRowView(parentView: @, model: person).toHtml()
```

As you can see, we've removed a good bit of code.  We've gone back to creating
our TableRowView in tableRow, but are now passing in reference to the
TableView as a parentView property.  And we are no longer telling TableRowView
when to render at all, instead we are broadcasting a "rendered" event that
gives anyone who cares a chance to do whatever they need to do.

We're also moving responsibility for when to render and what element to render
into the TableRowView.

```coffeescript
class Example.Views.TableRowView extends Backbone.View

  tagName: "tr"

  attributes: ->
    id: "row_#{@model.id}"

  constructor: (options) ->
    super
    @parentView = options.parentView
    @parentView.on "rendered", =>
      @setElement @parentView.$("#row_#{@model.id}")
      @render()

  toHtml: ->
    @$el.clone().wrap("<p>").parent().html()

  events:
    "click td": "clicked"

  clicked: ->
    alert "Way to go, you clicked a cell!"

  render: ->
    @$el.html JST["table_row_view_template"] @
```

It's not necessarily less code overall, but the parent view is much less
coupled to the child view.  The child view listens to the "rendered" event and
then finds her element within the parent's element.  And it seems to make
sense for the child to do this, after all she is one best equipped to know how
to locate her own element since she gave herself an id in the attributes
method.  In case you haven't seen this before, backbone will use tagName and
attributes to build the element for a view if you don't pass one in, which we
don't in the case of TableRowView.

The bit of this code that seems the least pleasant is the toHtml method.  This
is an unfortunate hack to get the outerHtml for the element, as jQuery doesn't
seem to provide a more convenient way to do it.  Gentle reader, feel free to
correct me if I'm wrong on this.

Overall though, I'm happier with this approach to composite views, so much so
that I've extracted a lot of this into a base view class in my
[backtastic](https://github.com/gaslight/backtastic) project.  I'll get into
lots more detail about backtastic in an upcoming post.
