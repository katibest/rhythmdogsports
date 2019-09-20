---
layout: post
post_author: Zack Kayser
current_gaslighter: true
categories:
- Development
tags:
- elixir
- refactoring
- development
- polymorphism
- protocols
- patterns
permalink: "/blog/:slug"
post_title: 'Refactoring Patterns in Elixir: Replace Conditional with Polymorphism
  Via Protocols Part 2'
publish_date: 2019-08-05 04:00:00 +0000
feature_post_image: ''
post_images: []
slug: refactoring-patterns-in-elixir-replace-conditional-with-polymorphism-via-protocols-part-2

---
## Part 2: Replace Conditionals with Polymorphism Via Protocols

_Use Elixir protocols to bring polymorphism to your data structures_

This is the second in a series of posts we are doing on refactoring patterns in Elixir, a series that stemmed from working through Martin Fowler’s book _Refactoring_. In the first part of this series, we looked at using pattern matching in function heads as a tool to use for refactoring complex conditionals.

Pattern matching is ubiquitous in Elixir and Erlang, and it is a great tool to use for cleaning up complex conditionals in most common cases. To recap what we did in the last post, let’s take a look at this code sample ported over to Elixir from the JavaScript example in Martin Fowler’s book, and then see how we were able to refactor it using pattern matching:

### The `**Bird**` module before refactoring:

    defmodule Bird do
    
    
      defstruct type: nil, number_of_coconuts: 0, voltage: 0
    
    
      def plumage(bird) do
        case bird.type do
          "EuropeanSwallow" ->
            "average"
          "AfricanSwallow" ->
            if bird.number_of_coconuts > 2 do
              "tired"
            else
              "average"
            end
          "NorwegianParrot" ->
            if bird.voltage > 100 do
              "scorched"
            else
              "beautiful"
            end
          _ ->
            "average"
        end
      end
    end
    

### The `**Bird**` module after refactoring:

    defmodule Bird do
      defstruct type: nil, number_of_coconuts: 0, voltage: 0
    
    
      def plumage(%__MODULE__{type: "AfricanSwallow", number_of_coconuts: num}) when num > 2, do: "tired"
    
    
      def plumage(%__MODULE__{type: "NorwegianParrot", voltage: voltage}) when voltage > 100, do: "scorched"
    
    
      def plumage(%__MODULE__{type: "NorwegianParrot"}), do: "beautiful"
    
    
      def plumage(%__MODULE__{}), do: "average"
    end
    

Just looking at the before and after versions of the above code snippet, we can see that pattern matching in function heads gives us a huge benefit and an easy win in the readability department. Our code is now easier to understand and reason about: with only four cases for the `**plumage/1**` function, we can easily find and determine what will be returned from the function for a given `**Bird**`.

We did, however, note one caveat to the refactoring tool used above: assuming we had a larger variety of `**Bird**`s to handle, with each variety requiring some unique logic to determine its plumage, we would naturally arrive at a situation where the shear amount of function heads that we would need to create would become overwhelming and lead to harder and harder to follow logic. Not only that, but since calls to these functions would fall through from top to bottom in the order they are declared, we would also have to think long and hard about what order to place the function heads in to arrive at the intended behavior. Eventually, we would reach a breaking point where maintaining this function would start to become intractable.

One solution for dealing this is actually quite simple: if we are dealing with a plethora of different kinds of `**Bird**` that all have different conditions that determine what their `**plumage**` is, then perhaps we should break away from the generic idea of a `**Bird**` and create data structures that are more specific and tailored to the kind of bird(s) we are actually working with. For example, given the code snippet above, maybe we want to have `**EuropeanSwallow**`, `**AfricanSwallow**`, and `**NorwegianParrot**` represented as data structures instead of just a `**Bird**` (or in addition to a `**Bird**` that would represent a general case).

Elixir actually gives us an elegant way of handling this kind of problem – protocols which you can read more about [**here**](https://elixir-lang.org/getting-started/protocols.html). Essentially, a protocol allows us to declare one function and delegate out the implementation of that based on the type of the data passed in to the function. Keeping with our `**Bird**` example, we might have a protocol `**Bird.plumage/1**` and delegate out specialized implementations of the function based on our `**EuropeanSwallow**`, `**AfricanSwallow**`, and `**NorwegianParrot**` data structures. Note that we can also have a fallback implementation for protocols so that we can handle generalized cases if that is what we need. Let’s take a look at what our protocol definition looks like:

    defprotocol Bird do
      @fallback_to_any true
      def plumage(bird)
    end
    

There are a couple things of note here. First, defining a protocol is more or less the same as defining a module (and, in fact, this will generate a module for us), and second, we define only function heads inside of the protocol without providing an implementation. The `**@fallback_to_any true**` declaration tells Elixir to reference an implementation of the function it marks for the `**Any**` data structure. We will look at an example of that below, but for now just note that you can use this to provide a generalized fallback implementation.

So once we have a protocol set up, how do we define our own custom implementations of the function(s) declared in the protocol? Let’s take a look at what our implementation would look like for the `**NorwegianParrot**`, and while we are at it, we will also create a module for `**NorwegianParrot**` and define a struct for it:

    defmodule NorweiganParrot do
      defstruct voltage: 0
    
    
      defimpl Bird, for: NorweiganParrot do
        def plumage(%NorweiganParrot{voltage: voltage}) when voltage > 100,
          do: "scorched"
    
    
        def plumage(_), do: "beautiful"
      end
    end
    

And for the `**EuropeanSwallow**` and `**AfricanSwallow**`, we would have something like:

    defmodule EuropeanSwallow do
      defstruct number_of_coconuts: 0
    
    
      defimpl Bird, for: EuropeanSwallow do
        def plumage(%EuropeanSwallow{}), do: "average"
      end
    end
    

and

    defmodule AfricanSwallow do
      defstruct number_of_coconuts: 0
    
    
      defimpl Bird, for: AfricanSwallow do
        def plumage(%AfricanSwallow{number_of_coconuts: num}) when num > 2,
          do: "tired"
    
    
        def plumage(_), do: "average"
      end
    

We are defining the specialized implementation for the three types of bird we are concerned with here. To do this, we use `**defimpl #{ProtocolName}, for: #{MyDataType}**` to let Elixir know that this is the implementation of the `**Bird**` protocol for the type we are passing as the value to `**for:**`. Just like in normal Elixir modules, we can use pattern matching and guard clauses when declaring functions, and we can also have multiple function heads for the same declaration. This is all just good ole classic Elixir.

The great thing about this is it gives us a much more focused, granular level of control over the way a function behaves based on the type of data it receives. We no longer have to worry about how a `**NorwegianParrot**`‘s plumage is calculated when we are working on our implementation of the `**AfricanSwallow**`.

As I mentioned above, we can also define a fallback implementation that will catch any parameters that are passed to `**Bird.plumage/1**` that don’t match the three specialized implementations we worked out above. You can do that by defining an implementation of the protocol for the `**Any**` data type like this:

    defimpl Bird, for: Any do
      def plumage(_), do: "average"
    end
    

With all of this in place, we can replace the initial code snippet from above with our protocol, giving us polymorphism on the parameters that get passed to `**Bird.plumage**` instead of relying on pattern matching in a single module and we can avoid potentially having to handle a large number of different cases in the same file. We now have more control over the behavior of the function and can write tests that can prove out different edge cases around the specialized implementations that we care about.

To finish this out, let’s look at what it might look like to call this code:

    1> Bird.plumage(%AfricanSwallow{number_of_coconuts: 7}) ### "tired"
    2> Bird.plumage("some general bird") ### "average"
    3> Bird.plumage(%NorwegianParrot{voltage: 7000}) ### "scorched"
    

That wraps up our second installment in our Refactoring Elixir Series. I hope you enjoyed, and happy Elixiring to you!