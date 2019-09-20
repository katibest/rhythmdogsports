---
layout: post
post_author: Zack Kayser
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'Refactoring Patterns in Elixir: Replace Conditional with Pattern Matching'
publish_date: 2019-04-15 13:58:00 +0000
feature_post_image: ''
post_images: []
slug: refactoring-patterns-in-elixir-replace-conditional-with-pattern-matching

---
## Part 1: Replace Conditionals with Pattern Matching

*Replace Complex Conditionals in Function Bodies with Pattern Matching in Function Heads*

Several of us developers at Gaslight have been meeting up on Fridays to discuss Martin Fowler's seminal book, [Refactoring](https://martinfowler.com/books/refactoring.html). The 2nd edition of his book, which is what we have been focused on, features examples written in JavaScript. The 1st edition, released in 2000, contained examples in Java. 

The first day we got together to discuss the book, an interesting question was raised: How do language paradigms and specific language implementations affect the way developers tackle the task of refactoring? For example, in cases where you might reach for a tool like inheritance in object-oriented languages, what would your alternative be if you were working in a functional language like Elixir or Erlang where tools such as inheritance are unavailable? On the flip side, if you were going to reach for a tool like pattern matching in Elixir, what would you reach for if you were coding in, say, Ruby? 

Since many of us here at Gaslight share a passion for Elixir, we started discussing if there was a set of "refactoring patterns" that we find ourselves coming back to when working on Elixir projects. It soon became obvious that there are plenty of techniques when it comes to refactoring Elixir, and we thought it would be great to put some of those down in writing and share them with the community.

Now, without any further ado, let's jump into our first refactoring pattern in Elixir:

### Replace Conditional with Function Head Pattern Matching

Our first pattern is a spin on one of the more influential refactoring techniques Martin Fowler brings up in his book: "Replace Conditional with Polymorphism". While this pattern does not, perhaps, fulfill what we might traditionally think of as polymorphism, it is a great option for refactoring complex conditionals in function bodies. We will also share another approach to refactoring conditionals using Elixir protocols -- an approach that gives you polymorphism on data structures -- in a future post. 

To demonstrate how this pattern looks, let's take a look at an example from _Refactoring_ ported over to Elixir:

```elixir
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
```

The example here is pretty straightforward: we pass in a `Bird` struct to the `plumage/1` function and pattern match on the type of bird in a case statement. Given the size of this function and its simplicity, it does not immediately jump out as being in dire need of refactoring, but there are a few "smells" that stick out if you have been working with Elixir for a while:

1. We are pattern matching on strings in the case statement. If new requirements come in that would force us to add more clauses to this case statement, this approach can get unwieldy in no time. 
2. We are using nested `if` blocks inside of matches in our case statement.
3. In the case of the `EuropeanSwallow` and the `AfricanSwallow` with less than 2 coconuts, we can just fall back to the default value of `average`. This would allow us to remove the "EuropeanSwallow" clause from the statement altogether, but we would still need to address the extra conditional for the `AfricanSwallow`.

So what are some solutions to clean this up a bit and make it easier to work with? _Let's extract the case statement conditional to pattern matching in function heads_:

```elixir
defmodule Bird do
  defstruct type: nil, number_of_coconuts: 0, voltage: 0

  def plumage(%__MODULE__{type: "AfricanSwallow", number_of_coconuts: num}) when num > 2, do: "tired"

  def plumage(%__MODULE__{type: "NorwegianParrot", voltage: voltage}) when voltage > 100, do: "scorched"

  def plumage(%__MODULE__{type: "NorwegianParrot"}), do: "beautiful"

  def plumage(%__MODULE__{}), do: "average"
end
```

Since Elixir and Erlang let you define multiple function heads for functions with the same arity, we can rely on pattern matching on our arguments and move more specific, conditional cases towards the top, leaving default and generic cases as the last definition(s) for a given function head. 

The use of guard statements in the function heads allows us to remove the two nested `if` blocks from the original case statement: we extract the data needed to determine whether or not a condition has been met using pattern matching while ignoring data that is irrelevant for our calculation. For instance, we only need to know how many coconuts the `AfricanSwallow` has to determine whether it's plumage is `tired` or `average`, so we can ignore the `voltage` property altogether.

The combination of pattern matching in our function heads along with guard clauses allows us to capture any `Bird` that doesn't match our conditional logic or any of our top function heads with a default case: `def plumage(%__MODULE__{}), do: "average"`. 

### When Should We Use `Replace Conditional with Function Head Pattern Matching`?

This pattern is a great approach for cleaning up `if/else` blocks and nested conditionals inside of function bodies. It also works well if all you are doing inside of a function body is running a case statement on a single property of a data structure, as is the case in our example above. 

### What Are Some Gotchas With This Pattern?

Elixir's pattern matching is a super powerful tool that gets used a lot, and rightfully so. It can make your code more declarative and easier to reason about, but you certainly _can_ get too carried away with it, and many Elixir/Erlang developers have probably been bitten by _overuse_ of pattern matching before.

To illustrate this point, imagine based on the example given above that we had a requirement to calculate a wide variety of plumages for *every single species of bird in existence on the planet*! Further complicating things, each species of bird has different attributes that determine their plumage. If you attempted to codify that using pattern matching in function heads in your `Bird` module, you would be in the fastlane on a one-way street to "What even in the world is going on here?!" 

Granted, the example I just gave is an argument ad absurdum, but I find that there is generally an upper limit on how many function heads you can match on before your code actually starts becoming _more difficult_ to read and understand. While I can't provide any specific advice on what that upper limit might be, I generally use this as a simple rule of thumb: If all of the function heads we are concerned with fit on the screen all at once, then our approach to pattern matching should work fine and the code will be easy to reason about. As we add more and more function heads, it becomes increasingly harder to reason about the code and thus less and less effective. In cases where this approach becomes unwieldy, you might want to try breaking out different data structures and having a protocol that has implementations for all of the data structures you need.

We will have a post coming up in the future that does just that: `Replace Conditional with Polymorphism: Elixir Protocols`. 

Thank you for reading and happy refactoring!