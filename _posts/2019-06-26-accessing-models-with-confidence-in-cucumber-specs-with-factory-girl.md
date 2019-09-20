---
layout: post
post_author: Joel Turnbull
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Accessing Models with Confidence in Cucumber Specs with factory_girl
publish_date: 2014-04-17T14:00:00.000+00:00
feature_post_image: ''
post_images: []
slug: accessing-models-with-confidence-in-cucumber-specs-with-factory-girl

---
We've mentioned our love of [page objects](http://gaslight.co/blog/6-ways-to-remove-pain-from-feature-testing-in-ruby-on-rails) in feature testing. Setting up the environment page objects, our pattern is to implement method missing to handle methods of type `_page`. When a step requests `login_page`, method missing picks up the request, lazy loads a `LoginPage` into a instance variable on demand. That `@login_page` instance is then available to any subsequent step in the spec through the `login_page` method.

````ruby
module ProjectWorld
  def method_missing(method, *args, &block)
    if method =~ /_page$/
      variable_name = "@#{method}"
      ivar = instance_variable_get(variable_name)

      unless ivar
        page = method.to_s.classify.constantize.new
        ivar = instance_variable_set(variable_name, page)
      end
      ivar
    else
      super
    end
  end
end
World(ProjectWorld)
````

This is possible because any instance variable that is set in the World of a Cucumber feature spec is globally available to any other step of that spec. This applies not only to page objects, but to any data models that are created or factory'd up into instance variables. We found ourselves doing things like this a lot in our steps.

````ruby
Given(/^there is an active promotion with title "(.*?)"$/) do |title|
  @active_promotion = FactoryGirl.create(:active_promotion, title: title)
end

Given(/^the active promotion has a message "(.*?)"$/) do |text|
  FactoryGirl.create(:message, promotion: @active_promotion, text: text)
end
````

The assumption in the second step makes makes me uncomfortable, i.e. some previous step has set up `@active_promotion`. Keep in mind these two steps could have been defined in completely different files, and still work. Global accessibility of instance variables in Cucumber seems wrong, but it can be rationalized I suppose. You can be relatively assured that each spec is a self contained little program with a relatively small scope.

We can avoid this in our data objects though by catching calls to them and lazy loading, similar to what we do with our page objects. Since we've given in to the idea of global state in our specs, why not apply this same pattern to our short-lived data models? Chris Nelson and I decided to give it a try on our last project. 

It turns out that factory_girl makes this super simple. factory_girl makes it easy to represent a model in it's various states by just setting it up and referring to it by name. For example it's easy and preferable in factory_girl to define representations for a promotion like this:

````ruby
FactoryGirl.create(:active_promotion)
FactoryGirl.create(:pending_promotion)
````

rather than:

````ruby
FactoryGirl.create(:promotion, status: "active")
FactoryGirl.create(:promotion, status: "pending")
````

This makes it easy define simple accessors in our Project world that return very specific, consistent data objects. Our module iterates through each defined factory, defining a lazy-loading accessor for each:

````ruby
module ProjectWorld
  def method_missing(method, *args, &block)
    ...
  end

  FactoryGirl.factories.map(&:name).each do |factory|
    define_method(factory) do
      variable_name = "@#{factory}"
      ivar = instance_variable_get(variable_name)

      unless ivar
        object = FactoryGirl.create(factory)
        ivar = instance_variable_set(variable_name, object)
      end
      ivar
    end
  end
end
````

We can now effectively change the above steps to this:

````ruby
Given(/^there is an active promotion with title "(.*?)"$/) do |title|	
end

Given(/^the active promotion has a message "(.*?)"$/) do |text|
  FactoryGirl.create(:message, promotion: active_promotion, text: text
end
````

The `active_promotion` method in the second step has been defined in our module. It will look for an instance_variable `@active_promotion`, and return it if it is defined. Otherwise it will set that `@active_promoiton` to a newly instantiated `active_promotion` factory. This effectively makes the first step, which was responsible for setting up the `@active_promotion`, a NOOP.

This pattern allows us to access models in the same way in every step, having confidence that it will never be nil.