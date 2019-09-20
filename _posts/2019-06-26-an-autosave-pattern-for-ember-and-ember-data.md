---
post_author: Mitch Lloyd
categories:
- Development
tags: []
post_title: An AutoSave Pattern for Ember and Ember Data
publish_date: 2013-06-19T15:11:00.000+00:00
feature_post_image: "/uploads/ember.png"
post_images: []
layout: post
current_gaslighter: false
slug: an-autosave-pattern-for-ember-and-ember-data
permalink: "/blog/:slug"

---
Requiring users to click a submit or save button in a web application is great
for developers but can be a burden on users in some situations. Sometimes it is
nice to automatically save the user's data as they fill out forms.

Automatically saving models in Ember can be a little tricky. A first pass might
look like this:

````coffeescript
App.Document = DS.Model.extend
  # some attributes here...

  saveWhenDirty: (->
    @get('store').commit() if @get('isDirty')
  ).observes('isDirty')
````

This solution has some significant problems:

  * Everytime the user types a character an ajax request is sent to the server
    resulting in a flood of requests.
  * As the user types, Ember throws errors saying that you can't modify
    attributes when a model is "inFlight".

## Debouncing Our Save

Let's tackle the "too many saves" problem first. Ideally we want to save the
user's data at some logical time. In many applications saving happens when a
form is submitted or when a field blurs. However handling all of the cases where
we want to save can get a little finicky. On their own, the built-in Ember controls work very reliably for updating model attributes. When I started extending these views to handle events where the user would want to save (e.g. focusOut, change) I ran into some edge cases where the events would not fire. For instance, when using the back button after editing a field I found that the change event would not fire on a field with focus.

In any event we also want to handle the case where a user is editing a lot of
text in a textarea and wants to save their work as they go. We want to save when
the user types.

This is a good case for a debounce function. Each time a debounce function is
called it resets a timer and waits to call another function. So in this case as
the user types, the debounce function is called repeatedly but only fires the
save function when the user pauses. Here is a simple debounce function based on
[underscore's debounce](http://underscorejs.org/#debounce).

````coffeescript
App.debounce = (func, wait) ->
  timeout = null

  ->
    context = this
    args = arguments

    lastArg = args[args.length - 1]
    immediate = true if lastArg and lastArg.now

    later = ->
      timeout = null
      func.apply(context, args)

    clearTimeout(timeout)

    if immediate
      func.apply(context, args)
    else
      timeout = setTimeout(later, wait)
````

One bonus feature here is that there is a way to clear out the pending function
call and execute immediately by passing `{now: true}` to our debounced function.

Let's use that function to implement some saner saving.

````coffeescript
App.DocumentController = Ember.ObjectController.extend
  autoSave: (->
    @debouncedSave()
  ).observes('content.body')

  debouncedSave: App.debounce (-> @save()), 1000

  save: ->
    @get('store').commit()
````

So now when the user stops typing for 1 second, the save function will be called.

## Give Our Attributes a Buffer

Even though we're saving less often, we'll still get errors from Ember if the
user stops typing for a second and then immediately starts typing again while
the record is saving.  To solve this problem we'll have users edit a buffer that
holds the attribute instead of editing it directly on the model. Since writing
this code, I've seen this pattern called a
[Buffered Proxy](https://gist.github.com/lukemelia/5632776).

````coffeescript
#= require ./debounce

BUFFER_DELAY = 1000

App.AutoSaving = Ember.Mixin.create
  # Setup buffers to write to instead of directly editing
  # the model attributes.
  _buffers: Ember.Map.create()

  # Buffered fields are saved after the set delay for the
  # debounce
  bufferedFields: []

  # InstaSave fields save the model as soon as they are changed
  instaSaveFields: []

  # Convenience property to access all the fields together
  _allFields: (->
    @get('bufferedFields').concat @get('instaSaveFields')
  ).property()

  # If we update a field that has been specified as one of the
  # bufferedFields or instaSaveFields write these to a buffer
  # instead of the actual attribute and save.
  setUnknownProperty: (key, value) ->
    if @get('bufferedFields').contains(key)
      @get('_buffers').set(key, value)
      @_debouncedSave()
    else if @get('instaSaveFields').contains(key)
      @_super(key, value)
      @_debouncedSave(now: true)
    else
      @_super(key, value)

  # Pull properties from the buffer if they have been set there.
  unknownProperty: (key) ->
    if @get('_allFields').contains(key) and @_buffers.get(key)
      @_buffers.get(key)
    else
      @_super(key)

  # Write the buffers to the actual content and save.
  _autoSave: ->
    if not @get('content.isSaving')
      @get('_buffers').forEach (value, key) =>
        @get('content').set(key, value)
      @get('content').save()
    else # If we're currently saving, then try to save later.
      @_debouncedSave()

  _debouncedSave: App.debounce (-> @_autoSave()), BUFFER_DELAY

  # When the model is about to change out from under the controller we must
  # immediately save any pending changes and clear out the buffers.
  _saveNowAndClear: (->
    return unless @get('content')
    @_debouncedSave(now: true)
    @set('_buffers', Ember.Map.create())
  ).observesBefore('content')
````

This mixin can be applied in a controller to provide a buffer between the Ember
fields and the model's attributes so that users can continue to type while
saving. Here is an example of this mixin in use.

````coffeescript
App.DocumentController = Ember.ObjectController.extend App.AutoSaving,
  bufferedFields: ['title', 'body']
  instaSaveFields: ['postedAt', 'category']
````

## Good Enough?

In practice this mixin has been working for me, but there is definitely some room
for improvement.  For one, it would be nice to let developers signal in the
templates rather than in the controller whether fields should be buffered or
instantly saved. This way there wouldn't be duplication of field names in the
controller and the template. One idea is to give fields special bindings like
`{% raw %}{{input value=bodyBuffered}}{% endraw %}` to determine how they should be handled.

Also I still see the possibility of losing data if the model `isSaving` when the
model changes out from under the controller. However, in practice I haven't been
able to produce this behavior.

Have you implemented something to handle autosaving? Have ideas for
improvements? Please let me know!
