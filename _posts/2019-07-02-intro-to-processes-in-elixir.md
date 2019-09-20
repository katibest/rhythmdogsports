---
layout: post
post_author: Scott Wiggins
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: Intro to Processes in Elixir
publish_date: 2019-06-17 15:00:00 +0000
feature_post_image: ''
post_images: []
slug: intro-to-processes-in-elixir

---
# Intro to Processes in Elixir


As I have been learning Elixir in the past few months, I have heard a lot about processes. Lots of
Elixir's biggest selling points, like its concurrency support, scalability, and fault tolerance, are
possible because of the features and characteristics of processes.


## Processing Processes


Elixir runs on the Erlang VM, and processes are a core part of Erlang's architecture. Erlang's
concurrency model relies on the idea of Actors. An Actor is a self-contained process that performs a
specific task and can only communicate with other processes via message-passing. Erlang's processes
are incredibly lightweight and fast, spinning up in MICROseconds. This small overhead also means you
can have a lot of them at once, up to 268 million.


Elixir provides a lot of incredibly useful abstractions on top of Erlang's processes, so you don't
usually need to interact with them directly, but I wanted to get in the weeds a little bit. So I
made a very simple Elixir application that utilizes some of the basics of spawning processes and
message-passing. The full source code for the project can be found
[here](https://github.com/willus10245/math_racer).


## Math Racer


MathRacer is a very simple and useful competitive addition application. It's public API is one
function: `MathRacer.race/1`. It takes an integer and spins up that number of `MathRacer.Adder`
processes, which then perform a complicated mathematical operation (1 + 2). The result and the time
it took to complete are printed to the console for each Adder, and at the end, the overall average
time to complete the operation is printed.


```elixir
iex(9)> MathRacer.race(10)
It took 67 microseconds to get the result: 3
:ok
It took 67 microseconds to get the result: 3
It took 67 microseconds to get the result: 3
It took 67 microseconds to get the result: 3
It took 92 microseconds to get the result: 3
It took 97 microseconds to get the result: 3
It took 97 microseconds to get the result: 3
It took 97 microseconds to get the result: 3
It took 97 microseconds to get the result: 3
It took 97 microseconds to get the result: 3
The average time was 84.5 microseconds
```


Let's start with the entry point of the application: `MathRacer.race/1`:


```elixir
defmodule MathRacer do
  def race(num_of_adders) when is_integer(num_of_adders) do
    range = 1..num_of_adders


    coordinator_pid =
      spawn(MathRacer.Coordinator, :loop, [[], Enum.count(range)])


    range
    |> Enum.each(fn _ ->
      adder_pid = spawn(MathRacer.Adder, :loop, [])
      send(adder_pid, {coordinator_pid, {:add, [1,2]}, :erlang.timestamp()})
    end)
  end
end
```


Here we see two of the functions that Elixir gives you for dealing directly with processes:
`spawn/3` and `send/2`. `spawn/3` is used to start a process. It takes a module, and atom
representing a function in that module, and a list of arguments. It starts a process by calling the
given function with list of arguments. The spawn function returns the PID (process identifier) of
the process, which is placed into a variable for later use. Two kinds of processes are spawned in
this function. One calls the `loop/2` function of the `MathRacer.Coordinator` module with an empty
list and the length of our range as arguments. The second calls the `loop/0` function of the
`MathRace.Adder` module with no arguments. We'll look at what these functions do later.


The other important function used here is `send/2`. This is how messages are sent between processes.
It is called with the PID of the process you are sending a message to, as well as the message
itself.


Our `race/1` function takes the number passed to it, spawns that many `MathRacer.Adder` processes
and sends them each a message in the form of a tuple containing the PID of the coordinator process,
another tuple contianing the atom `:add` and a list of two numbers, and a timestamp taken using a
function from the core Erlang module.


Let's take a look at the other side of this message sending equation. The core of our application
is the `MathRacer.Adder` module.


```elixir
defmodule MathRacer.Adder do
  def loop do
    receive do
      {sender_pid, {:add, [a, b]}, start_time} ->
        send(sender_pid, add(a, b, start_time))
      _ ->
        :error
    end
  end


  def add(a, b, start_time) when is_number(a) and is_number(b) do
    {:ok, a + b, :timer.now_diff(:erlang.timestamp(), start_time)}
  end
end
```


This module contains two functions: `add/3` and `loop/0`. `add/3` is a simple function that takes
two numbers and a timestamp, and returns the sum of the two numbers and the difference between the
time it receives and the time it executes. This utilizes the `timer` module from Erlang. The
`now_diff/2` function takes two Erlang timestamps and returns the difference in microseconds.


The `loop/0` function is the interesting part. It contains a `receive` block. This is how an Elixir
process defines the messages it can receive. Using pattern matching, you list the kinds of messages
that your process can receive and give a default response for any unknown messages. The rest is
simple. If you send `MathRacer.Adder` the message it expects, it responds by sending the PID given
to it a message containing the result of the `add/3` function call.


The last piece to look at is the Coordinator module.


```elixir
defmodule MathRacer.Coordinator do
  def loop(results \\ [], results_expected) do
    receive do
      {:ok, sum, time} when is_number(sum) and is_number(time) ->
        IO.puts("It took #{time} microseconds to get the result: #{sum}")
        new_results = [time | results]
        if results_expected == Enum.count(new_results) do
          send self(), :exit
        end
        loop(new_results, results_expected)
      :exit ->
        average = Enum.sum(results) / Enum.count(results)
        IO.puts("The average time was #{average} microseconds")
      _ ->
        loop(results, results_expected)
    end
  end
end
```


This module just contains a `loop/2` function. Inside, we can see some similarities to the `loop/0`
funcion in `MathRacer.Adder`, with a few more possible messages defined. `loop/2`'s arguments are a
list, which will be used to build up a list of all the results it receives, and a number
representing how many results it should expect to receive. Notice that the `loop/2` function is
recursive. Two of the message options end by calling the loop function again. This is because the
coordinator process has a different role than the adder processes. Each Adder process does
its calculation and returns, then exits. If you need to add again, you need to spawn another
process. The coordinator process, on the other hand, needs to hang around and wait for every adder's
result to come in. In order to accomplish this, it must call it self once it receives a message in
order to keep listening for the next one.


When the Coordinator receives a message from an Adder, it first prints a message to the console
informing you of the time it took that Adder to get its result. It then adds the new result to its
ongoing list of results and checks to see if its waiting on any more. If it is, the loop function
calls itself with the new list of results. If it has received all its expected messages, the
coordinator process actually sends itself a message, consisting of the atom `:exit`. Processes can
send themselves messages. This is useful for isolating unrelated logic instead of mixing concerns
into the same function. When the Coordinator receives an exit message, it takes its list of results,
calculates the average time, and prints that to the console and exits.


## send(self(), :wrap_it_up)


So that's a (not so) brief overview of some of the basics of dealing with processes in Elixir.
There's a lot more to look at. We haven't covered linking or monitoring a process, which is how you
can have things like Supervisors and other awesome Elixir functionality. But I hope this got you
interested in digging more into how Elixir works under the hood and how it does the awesome things
it does.