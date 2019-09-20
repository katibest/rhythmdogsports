---
layout: post
post_author: Chris Nelson
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'Microservices in Phoenix: Part 2'
publish_date: 2016-02-29 19:00:00 +0000
feature_post_image: ''
post_images: []
slug: microservices-in-phoenix-part-2

---
In the [Part 1](http://teamgaslight.com/blog/microservices-in-phoenix-part-1) of this series I talked about how to take a Phoenix application and break off bits into Microservices. I got as far along as extracting some code from a Phoenix controller into an OTP server. In this installment, we'll take the next logical step and pull our OTP server out into a whole separate OTP application.

Before I get into the code, though, I thought it might be useful to step back and define the term Microservices and talk about what makes a good candidate for extracting.

![You keep using that word](https://gaslight-blog.s3.amazonaws.com/microservices-in-phoenix-part-2/that_word.jpg)

Here's what I mean when I use the word: a bit of functionality that can be separately deployed and scaled independently of the main application. Notice what I'm *not* putting in this definition: anything about transport mechanisms, data formats, or protocol. I don't care a bit for my purposes about HTTP, JSON, or XML. With this definition, OTP servers (and applications) fit the bill perfectly. They are separate processes with their own state and can be deployed and scaled separately. OTP applications in particular give me really powerful tools for managing all this.

In the case of our imaginary, oversimplified, auction application receiving bids and notifying connected clients of a new bid make a lot of sense as a candidate for extracting into a microservice. We can scale out our system by having a process (or processes) manage each item potentially. This is probably as granular as we want to get: having the management of which bid is winning gets complicated to distribute. It's ideal to have a single process be in charge of deciding who the winner is.

Let's shift gears back to our code. We have our OTP server, and now we're ready to take the next step and move it into a separate application. I found doing this to be surprisingly simple. We start by creating a new application by running:

```
mix auction_server --sup
```

This creates a new elixir application (note that is *not* a Phoenix app). The code for this extracted OTP app is in this [repo](https://github.com/gaslight/auction_server). The `--sup` flag tells mix to create our OTP application with a supervisor. I'm not going to cover supervisors in any great detail here, but suffice to say this is the entry point to our microservice and is responsible for starting and managing whatever processes we need to run. Here's the supervisor that mix will generate for us:

```elixir
defmodule AuctionServer do
  use Application

  # See http://elixir-lang.org/docs/stable/elixir/Application.html
  # for more information on OTP Applications
  def start(_type, _args) do
    import Supervisor.Spec, warn: false

    children = [
      # Define workers and child supervisors to be supervised
      # worker(AuctionServer.Worker, [arg1, arg2, arg3]),
    ]

    # See http://elixir-lang.org/docs/stable/elixir/Supervisor.html
    # for other strategies and supported options
    opts = [strategy: :one_for_one, name: AuctionServer.Supervisor]
    Supervisor.start_link(children, opts)
  end
end
```

Note the empty list of children. That's where we'll put the processes we want to have supervised. Let's look back at our main application. The main thing we want to move is AuctionServer. We can start by simply moving this file into our newly created application. However, I caused a slight problem for us when I named our new application AuctionServer. Let's fix this by renaming our server process AuctionServer.BidServer. The result looks something like this:

```elixir
defmodule AuctionServer.BidServer do
  use GenServer

  alias AuctionServer.Repo
  alias AuctionServer.Bid

  # Client API

  def start_link(opts \\ []) do
    {:ok, pid} = GenServer.start_link(__MODULE__, [], opts)
  end

  def new_bid(bid_params) do
    GenServer.call(:bid_server, {:new_bid, bid_params})
  end

  # Server implementation

  def init([]) do
    bids = Repo.all(Bid)
    {:ok, bids}
  end

  def handle_call({:new_bid, bid_params}, _from, bids) do
    changeset = Bid.changeset(%Bid{}, bid_params)
    case Repo.insert(changeset) do
      {:ok, bid} ->
        Auctioneer.Endpoint.broadcast! "bids:max", "change", Auctioneer.BidView.render("show.json", %{bid: bid})
        {:reply, {:ok, bid}, [bid | bids]}
      {:error, changeset} ->
        {:reply, {:error, changeset}, bids}
    end
  end

end
```

It's pretty much exactly the same as before other than the rename. At the top of the file, notice those two aliases: they refer to Bid struct and the Repo that persists Bids to the database. We'll want to move both of those into our new application as well. Doing so is as simple as copying the files and renaming `Auctioneer` to `AuctionServer`.

We'll also need to add `ecto` and `postgrex` to our list of dependencies. We do this in `mix.exs`:

```elixir
...
  defp deps do
    [
      {:ecto, "> 0.0.0"},
      {:postgrex, "> 0.0.0"}
    ]
  end
end
```

Now we're ready to tell our supervisor how to start our processes. We'll need to start both our Repo process to manage database connectivity and our BidServer. We do this by adding them to our list of workers:

```elixir
...
    children = [
      # Define workers and child supervisors to be supervised
      # worker(AuctionServer.Worker, [arg1, arg2, arg3])
      worker(AuctionServer.Repo, []),
      worker(AuctionServer.BidServer, [[name: :bid_server]])
    ]

    # See http://elixir-lang.org/docs/stable/elixir/Supervisor.html
    # for other strategies and supported options
    opts = [strategy: :one_for_one, name: :auction_server_supervisor]
...
```

I've added a name for BidServer to make it a little easier to find in observer. I changed the name of the supervisor in `opts` for the same reason.

Next, we should be ready change our original Phoenix app, Auctioneer, to use our AuctionServer app. We do this by adding it to mix.exs, both as a dependency:

```elixir
defp deps do
  [
    {:ex_machina, "~> 0.5"},
    {:plug_cors, "~> 0.7.3"},
    {:phoenix, "~> 1.0.2"},
    {:phoenix_ecto, "~> 1.1"},
    {:postgrex, ">= 0.0.0"},
    {:phoenix_html, "~> 2.1"},
    {:phoenix_live_reload, "~> 1.0", only: :dev},
    {:cowboy, "~> 1.0"},
    {:auction_server, path: "../auction_server", env: Mix.env}
  ]
end
```

And also to our list of applications:

```elixir
def application do
  [mod: {Auctioneer, []},
   applications: [:ex_machina, :phoenix, :phoenix_html, :cowboy, :logger,
                  :phoenix_ecto, :postgrex, :auction_server]]
end
```

This will both bring in the code for our new application and ensure that its supervision tree is started. We're ready to make the change in our controller next. In `BidController` we should just be able to change the call to `AuctionServer` to `AuctionServer.BidServer`.

```elixir
def create(conn, %{"bid" => bid_params}) do
  case AuctionServer.BidServer.new_bid(bid_params) do
    {:ok, bid} ->
      conn
      |> put_status(:created)
      |> put_resp_header("location", bid_path(conn, :show, bid))
      |> render("show.json", bid: bid)
    {:error, changeset} ->
      conn
      |> put_status(:unprocessable_entity)
      |> render(Auctioneer.ChangesetView, "error.json", changeset: changeset)
  end
end
```

At this point, we should be good to go. If we launch our main phoenix app it should launch our AuctionServer application as well. We can see this in the observer if we bring up the applications tab:

![observer_screenshot1](https://gaslight-blog.s3.amazonaws.com/microservices-in-phoenix-part-2/observer.png)

Sure enough, we see our auction_server in the list of applications and when we highlight it, we see only the processes for the auction_server OTP app, not for the main auctioneer phoenix app. Other than that, things should work as before.

Well, we've come to the end of our journey for now. There's plenty more we could do from here: we still have some calls to `Auctioneer` from `AuctionServer.BidServer`. It would be nice to refactor those out to make our microservice more independent. It's also worth pointing out that there is a handy feature in mix for creating projects that consist of multiple OTP apps: the umbrella project. Because I deliberately wanted to show what it looks like to pull an OTP app after the fact I chose not to use this feature, but it would have made some things even easier.

I hope you've enjoyed following along. We're excited about Elixir and Phoenix, and if you'd like to learn more about both of these great technologies, we'll be in [San Francisco March 21-23 doing a three-day, hands-on class.](https://teamgaslight.com/training/courses/25-building-next-gen-web-apps-with-elixir-and-phoenix) We'd love to see you there!

