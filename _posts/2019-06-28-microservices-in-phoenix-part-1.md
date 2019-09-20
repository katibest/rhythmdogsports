---
layout: post
post_author: Chris Nelson
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'Microservices in Phoenix: Part 1'
publish_date: 2016-01-20 12:00:00 +0000
feature_post_image: ''
post_images: []
slug: microservices-in-phoenix-part-1

---
Last month at CodeMash I gave a talk entitled "Low Ceremony Microservices with Elixir." I spent a lot of the talk working through an example auction application in Phoenix. During the talk, I started with a "Monolith" with all of the code directly in the main Phoenix project and gradually extracted a key bit into a separate OTP application that lives in a separate mix project. I hadn't seen any examples of how to do this, but I found it surprisingly easy to do and thought it would make a useful blog post.

In my talk, I worked through a definitely oversimplified auction site. I separated the front end (an Angular 2 app) from the backend, and in this series, I'll just be focusing on the Phoenix backend. All the code is in this [repo](https://github.com/gaslight/auctioneer), and I've created branches for each step along the way. To follow along, you can checkout the `start` branch. Let's take a look at BidController, and specifically, the create method.

```elixir
def create(conn, %{"bid" => bid_params}) do
  changeset = Bid.changeset(%Bid{}, bid_params)

  case Repo.insert(changeset) do
    {:ok, bid} ->
      Endpoint.broadcast! "bids:max", "change", Auctioneer.BidView.render("show.json", %{bid: bid})
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

This is pretty standard Phoenix code, the only thing I've added from the output of the `mix phoenix.gen.json` is the broadcast code. This just lets all the connected clients know that a new bid has occurred, so they can display immediate feedback to the user. This works fine, but as our application grows in complexity and scale, having the auction managed as a separate process might make a lot of sense. Let's do that one step at a time.

To begin with, we'll create a module that `Auctioneer.AuctionServer` that implements a `GenServer`. I've done this in the `gen_server` branch.

```elixir
defmodule Auctioneer.AuctionServer do
  use GenServer

  alias Auctioneer.Repo
  alias Auctioneer.Bid

  # Client API

  def start_link(opts \\ []) do
    {:ok, pid} = GenServer.start_link(__MODULE__, [], opts)
  end

  def new_bid(bid_params) do
    GenServer.call(:auction_server, {:new_bid, bid_params})
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

As is customary in a GenServer, I've broken out the client API from the server implementation. These two sets of methods are going to be called from different processes: The client API will be called from our controller process while the server implementation will be called from the AuctionServer process itself.

GenServer processes can track state, and we setup the initial state for AuctionServer by looking up all the Bids from the database. Note this is a deliberately oversimplified 1 item auction or we'd have to be more specific! We change our state by adding return a list with the new bid added to the head as the last element of our tuple.

The code to call the AuctionServer is pretty straightforward, too:

```elixir
def create(conn, %{"bid" => bid_params}) do
  case Auctioneer.AuctionServer.new_bid(bid_params) do
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
```

Because we've had `new_bid` return the same response tuple that Repo.create was, most of the code is unchanged.

Notice we're not calling `start_link` ourselves. This is because our main Phoenix OTP application can take care of this for us. We do this by adding a new worker in `lib/auctioneer.ex`.

```elixir
defmodule Auctioneer do
  use Application

  # See http://elixir-lang.org/docs/stable/elixir/Application.html
  # for more information on OTP Applications
  def start(_type, _args) do
    import Supervisor.Spec, warn: false

    children = [
      # Start the endpoint when the application starts
      supervisor(Auctioneer.Endpoint, []),
      # Start the Ecto repository
      worker(Auctioneer.Repo, []),
      worker(Auctioneer.AuctionServer, [[name: :auction_server]])
      # Here you could define other workers and supervisors as children
      # worker(Auctioneer.Worker, [arg1, arg2, arg3]),
    ]

    # See http://elixir-lang.org/docs/stable/elixir/Supervisor.html
    # for other strategies and supported options
    opts = [strategy: :one_for_one, name: Auctioneer.Supervisor]
    Supervisor.start_link(children, opts)
  end

  # Tell Phoenix to update the endpoint configuration
  # whenever the application is updated.
  def config_change(changed, _new, removed) do
    Auctioneer.Endpoint.config_change(changed, removed)
    :ok
  end
end
```

We've added a new worker call for our AuctionServer and given it a name to make it easy to see it in the observer. If you start the app at this branch with `iex -S mix phoenix.server` you can run `:observer.start` in the iex session and see the process tree:

![observer_screenshot1](https://gaslight-blog.s3.amazonaws.com/microservices-in-phoenix-part-1/observer_screenshot.png)

We're not making much change to how creating bids works yet, but it's interesting to think about the possibilities that moving this code into a separate process gives us. As a long-time Rails developer, I hadn't really given much thought to the database being the holder of state for a web app. It's just kind of what you do. But by making state more explicit, we have more choices. For example, we are still immediately persisting the Bid to the database, but there's no reason we have to as the state is maintained by the server. We'd need to save off all the Bids when this process terminates, but this could be handled by implementing `terminate` or with a supervisor.

Having a separate process for the AuctionServer is great, but we've still got all our code in the same project. What if our AuctionServer gets interesting enough to need a project of it's own? It turns out, extracting AuctionServer into a separate OTP application is really easy to do. I'll be [tackling this in the next installment.](https://teamgaslight.com/blog/microservices-in-phoenix-part-2)

If you happen to be in San Francisco, I'm teaching a 3-day class called ["Building Next Gen Web Apps With Elixir and Phoenix" March 21-24.](https://teamgaslight.com/training/courses/25-building-next-gen-web-apps-with-elixir-and-phoenix)


