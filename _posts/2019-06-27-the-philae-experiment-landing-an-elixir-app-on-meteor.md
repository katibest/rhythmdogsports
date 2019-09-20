---
layout: post
post_author: James Smith
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'The Philae Experiment: Landing an Elixir App on Meteor'
publish_date: 2015-01-27 19:22:00 +0000
feature_post_image: "/uploads/Philae_2.png"
post_images: []
slug: the-philae-experiment-landing-an-elixir-app-on-meteor

---
A few months ago [Super Chris Nelson](https://teamgaslight.com/people/chris-nelson) challenged me to use Elixir to talk to a Meteor application because “It sounds like something Elixir would be good at.” 

At the time a few local developers&#8212;Doug Rohrer, Chris Nelson, Paul Henrich, Jason Voegele and I&#8212;had been meeting on a regular basis to [hack on Elixir.](https://teamgaslight.com/blog/the-best-resources-for-learning-elixir) We had been working on a Kafka adapter for Elixir but, after a client project moved away from Kafka, we decided to set the project aside. 

So I pitched the idea of working on a DDP client for Elixir. I already had a pretty simple working example of sending a connect message to the basic leaderboard example app in Meteor and getting a response back. Around this time, humanity had managed to land a rover named Philae on a comet! So naming our little experiment Philae made sense.

The leaderboard app in Meteor has served as our example app for working on Philae, so we will be connecting to it here. 

I'm going to walk you through the basic process we used to connect to a Meteor app using our in-progress library Philae and build a small example Elixir app along the way.

###Getting Started

I assume you have both Elixir and Meteor installed. If not, you can find install instructions here:

[https://www.meteor.com/install](https://www.meteor.com/install)
<br/>
[http://elixir-lang.org/install.html](http://elixir-lang.org/install.html)


### Creating a New Elixir Project
```
mix new philae_example
cd philae_example
```

### Adding Dependencies
Next you will need to add Philae to your mix dependencies. For now you will need to add Philae’s as a Git dependency. We plan to eventually release the library as a hex package. 

####mix.exs
```elixir
defmodule PlayerVoter.Mixfile do
  use Mix.Project

  def project do
    [app: :player_voter,
     version: "0.0.1",
     elixir: "~> 1.0",
     deps: deps]
  end

  # Configuration for the OTP application
  #
  # Type `mix help compile.app` for more information
  def application do
    [applications: [:logger]]
  end

  # Dependencies can be Hex packages:
  #
  #   {:mydep, "~> 0.3.0"}
  #
  # Or git/path repositories:
  #
  #   {:mydep, git: "https://github.com/elixir-lang/mydep.git", tag: "0.1.0"}
  #
  # Type `mix help deps` for more examples and options
  defp deps do
    [{:philae, git: "https://github.com/cincinnati-elixir/philae.git"}]
  end
end
```

Next we will fetch and compile our new dependencies:

```
mix deps.get
mix deps.compile

```

### Installing a Meteor Example App
Now let's install the Meteor leaderboard example app:

```
meteor create --example leaderboard
```

From a separate terminal go ahead and run the new Meteor app:

```
cd leaderboard
meteor 
```

You should see the meteor app is now running:

```
 ~/src/elixir/philae_example/leaderboard $ meteor
[[[[[ ~/src/elixir/new_philae_example/leaderboard ]]]]]

=> Started proxy.
=> Started MongoDB.
=> Started your app.

=> App running at: http://localhost:3000/

```
Visit http://localhost:3000 and you should see a basic leaderboard.

![Meteor Leaderboard example](http://i.imgur.com/ScdnkYn.png)


### Creating a Philae Handler Module

Now let's create an Elixir module to connect to the leaderboard app. This will be a basic GenServer and will also use the DDPHandler behavior provided by Philae. You can find out more about GenServers in the Elixir Getting started guide [http://elixir-lang.org/getting_started](http://elixir-lang.org/getting_started)


philae_example/lib/leader.ex

```elixir
defmodule Leader do
  use DDPHandler
  use GenServer

  def start_link do
    GenServer.start_link(__MODULE__, [], [])
  end

  def init([]) do
    {:ok, client_pid } = Philae.DDP.connect("ws://localhost:3000/websocket", __MODULE__, self)
  end
end
```

This gives us everything we need to connect to our Meteor app and get some messages back over [DDP](https://www.meteor.com/ddp).

Next let's start the Elixir REPL with our application loaded and make a connection to the Leaderboard app.

```
 iex -S mix 
 Leader.start_link
```

After this you should see a whole lot of output in your terminal:

```
¶ ~/src/elixir/new_philae_example $ iex -S mix
Erlang/OTP 17 [erts-6.2] [source] [64-bit] [smp:8:8] [async-threads:10] [hipe] [kernel-poll:false] [dtrace]

Compiled lib/new_philae_example.ex
lib/leader.ex:10: warning: variable client_pid is unused
Compiled lib/leader.ex
Generated new_philae_example.app
Interactive Elixir (1.0.2) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)> Leader.start_link

20:32:24.483 [info]  WS In: {"server_id":"0"}
{:ok, #PID<0.163.0>}

20:32:24.501 [info]  Unhandled message received %{"server_id" => "0"}

20:32:24.501 [info]  WS Out: {"version":"1","support":["1","pre2","pre1"],"msg":"connect"}
iex(2)>
20:32:24.502 [info]  WS In: {"msg":"connected","session":"D3zmeLHrLYdCeQKCz"}

20:32:24.502 [info]  In: %{"msg" => "connected", "session" => "D3zmeLHrLYdCeQKCz"}

20:32:24.502 [info]  WS In: {"msg":"added","collection":"players","id":"yiPp9GZpNoKcd8hjY","fields":{"name":"Ada Lovelace","score":45}}

20:32:24.504 [info]  In: %{"collection" => "players", "fields" => %{"name" => "Ada Lovelace", "score" => 45}, "id" => "yiPp9GZpNoKcd8hjY", "msg" => "added"}

20:32:24.504 [info]  WS In: {"msg":"added","collection":"players","id":"JPwJ2Pda5DWz5NSPJ","fields":{"name":"Grace Hopper","score":35}}

20:32:24.504 [info]  In: %{"collection" => "players", "fields" => %{"name" => "Grace Hopper", "score" => 35}, "id" => "JPwJ2Pda5DWz5NSPJ", "msg" => "added"}

20:32:24.504 [info]  WS In: {"msg":"added","collection":"players","id":"ups8ufNNKAvz9wSXZ","fields":{"name":"Marie Curie","score":45}}

20:32:24.504 [info]  In: %{"collection" => "players", "fields" => %{"name" => "Marie Curie", "score" => 45}, "id" => "ups8ufNNKAvz9wSXZ", "msg" => "added"}

20:32:24.504 [info]  WS In: {"msg":"added","collection":"players","id":"x49dhtpx6MYNqW7ps","fields":{"name":"Carl Friedrich Gauss","score":5}}

20:32:24.504 [info]  In: %{"collection" => "players", "fields" => %{"name" => "Carl Friedrich Gauss", "score" => 5}, "id" => "x49dhtpx6MYNqW7ps", "msg" => "added"}

20:32:24.504 [info]  WS In: {"msg":"added","collection":"players","id":"ysBJnxWu7d4cbhseQ","fields":{"name":"Nikola Tesla","score":45}}

20:32:24.505 [info]  In: %{"collection" => "players", "fields" => %{"name" => "Nikola Tesla", "score" => 45}, "id" => "ysBJnxWu7d4cbhseQ", "msg" => "added"}

20:32:24.505 [info]  WS In: {"msg":"added","collection":"players","id":"dQcPFWRjRKhfGXy48","fields":{"name":"Claude Shannon","score":45}}

20:32:24.505 [info]  In: %{"collection" => "players", "fields" => %{"name" => "Claude Shannon", "score" => 45}, "id" => "dQcPFWRjRKhfGXy48", "msg" => "added"}

20:32:54.503 [info]  WS In: {"msg":"ping"}

20:32:54.503 [info]  WS Out: {"msg":"pong"}
```

What does all of this mean? 

### Interpreting DDP Messages
First we can see that we sent out a connect message over WebSocket:


```
20:32:24.501 [info]  WS Out: {"version":"1","support":["1","pre2","pre1"],"msg":"connect"}
```

Then followed by a message back in over Websocket

```
20:32:24.502 [info]  WS In: {"msg":"connected","session":"D3zmeLHrLYdCeQKCz"}
```

This tells us we are now connected to the Meteor app, and we get an id for our session that we will want to keep track of.

We then get another nearly identical message in our log:

```
20:32:24.502 [info]  In: %{"msg" => "connected", "session" => "D3zmeLHrLYdCeQKCz"}
```

Why do we get the same message twice? Well this is the message that our Leader GenServer received from the Philae.DDP GenServer. It's messages, processes and GenServers all the way down!

Next we see some more useful messages in our log telling us that some players were added to a collection called 'players'.

```
20:32:24.502 [info]  WS In: {"msg":"added","collection":"players","id":"yiPp9GZpNoKcd8hjY","fields":{"name":"Ada Lovelace","score":45}}

20:32:24.504 [info]  In: %{"collection" => "players", "fields" => %{"name" => "Ada Lovelace", "score" => 45}, "id" => "yiPp9GZpNoKcd8hjY", "msg" => "added"}
``` 
Once again we can see we logged a message coming into the WebSocket, as well as the message we received in our Leader GenServer process.

Why are we receiving these when we haven't subscribed to the player collection yet? 
By default Meteor includes a package called autopublish. This is something you want to remove before publishing an app, so let's go ahead and remove it.

```
cd leaderboard
meteor remove autopublish
```
Make sure meteor is running again and then connect to our app again:

```
cd ../
iex -S mix
Leader.start_link
```
Now you can see Meteor is a lot less chatty:

```
iex(1)> Leader.start_link

20:52:10.047 [info]  WS In: {"server_id":"0"}
{:ok, #PID<0.148.0>}

20:52:10.065 [info]  Unhandled message received %{"server_id" => "0"}
iex(2)>
20:52:10.065 [info]  WS Out: {"version":"1","support":["1","pre2","pre1"],"msg":"connect"}

20:52:10.065 [info]  WS In: {"msg":"connected","session":"bZFji7PAuFBR34aAu"}

20:52:10.066 [info]  In: %{"msg" => "connected", "session" => "bZFji7PAuFBR34aAu"}
```

### Subscribing to Meteor Collections
Let's go ahead and get connected to the player collection again.

Open up lib/leader.ex and change the init function it to look like the following:

```elixir
  def init([]) do
    {:ok, client_pid } = Philae.DDP.connect("ws://localhost:3000/websocket", __MODULE__, self)
    {collection, id} = Philae.DDP.subscribe(client_pid, "players")
    {:ok, %{client_pid: client_pid, subscription_id: id, collection: collection}}
  end
```

Ok what is happening in this function? 

First we are going to connect to our Meteor app. This time, however, we are going to capture the client pid we get back, which is the process id to the socket.

Next we call the subscribe function passing in the client_id we captured as the first parameter and the name of the collection we want to subscribe to, in this case "players".

We match the collection and id that we get back from the call to the subscribe function and return them along with the client_pid as the state for our GenServer.

Now if we restart our iex session (by typing ctrl-c twice) and start our Leader GenServer, we get something scary. A 404 error message that looks like this. Yikes.

```
1:14:43.265 [info]  WS Out: {"name":"players","msg":"sub","id":"0c30cb41-bfeb-41f4-852e-efc3c985388e"}
iex(2)>
21:14:43.266 [info]  WS In: {"msg":"nosub","id":"0c30cb41-bfeb-41f4-852e-efc3c985388e","error":{"error":404,"reason":"Subscription not found","message":"Subscription not found [404]","errorType":"Meteor.Error"}}
```

This is because Meteor can't find our subscription. When we removed autopublish, we didn't tell Meteor what to publish. Let's go ahead and tell it to publish the "players" collection. 

Open up leaderboard/leaderboard.js and add following at the bottom of the file:

```javascript
Meteor.publish("players", function (){
    return Players.find();
  });
```
Add a call to subscribe to line to the end of isClient section on line 34:

```javascript
  Meteor.subscribe("players");
```

So your leadboard.js should look like this:

```javascript
// Set up a collection to contain player information. On the server,
// it is backed by a MongoDB collection named "players".

Players = new Mongo.Collection("players");

if (Meteor.isClient) {
  Template.leaderboard.helpers({
    players: function () {
      return Players.find({}, { sort: { score: -1, name: 1 } });
    },
    selectedName: function () {
      var player = Players.findOne(Session.get("selectedPlayer"));
      return player && player.name;
    }
Meteor.subscribe("players");
  });

  Template.leaderboard.events({
    'click .inc': function () {
      Players.update(Session.get("selectedPlayer"), {$inc: {score: 5}});
    }
  });

  Template.player.helpers({
    selected: function () {
      return Session.equals("selectedPlayer", this._id) ? "selected" : '';
    }
  });

  Template.player.events({
    'click': function () {
      Session.set("selectedPlayer", this._id);
    }
  });
}

// On server startup, create some players if the database is empty.
if (Meteor.isServer) {
  Meteor.startup(function () {
    if (Players.find().count() === 0) {
      var names = ["Ada Lovelace", "Grace Hopper", "Marie Curie",
                   "Carl Friedrich Gauss", "Nikola Tesla", "Claude Shannon"];
      _.each(names, function (name) {
        Players.insert({
          name: name,
          score: Math.floor(Random.fraction() * 10) * 5
        });
      });
    }
  });
  Meteor.publish("players", function (){
    return Players.find();
  });
}
```
 Restart your iex session and Leader GenServer again and we should now get back messages from the player collection once more.

```
iex(1)> Leader.start_link

...

{:ok, #PID<0.157.0>}
iex(2)>
21:20:22.019 [info]  WS In: {"msg":"added","collection":"players","id":"yiPp9GZpNoKcd8hjY","fields":{"name":"Ada Lovelace","score":45}}

21:20:22.021 [info]  In: %{"collection" => "players", "fields" => %{"name" => "Ada Lovelace", "score" => 45}, "id" => "yiPp9GZpNoKcd8hjY", "msg" => "added"}
```

### Receiving Updates from Meteor Collections
Now with both your iex session and your meteor app running,  visit the Meteor app in your browser http://localhost:3000. This time let's vote for Ada and see what happens.

Looking at the iex session you should see a new set of messages:

```
21:33:17.080 [info]  WS In: {"msg":"changed","collection":"players","id":"yiPp9GZpNoKcd8hjY","fields":{"score":50}}

21:33:17.080 [info]  In: %{"collection" => "players", "fields" => %{"score" => 50}, "id" => "yiPp9GZpNoKcd8hjY", "msg" => "changed"}
```

Now we can see that we are receiving messages whenever items in the collection are modified. Awesome!

### Making RPC Calls to Meteor and Trivial Voter Fraud
This is cool but what can we do with it? So far we have mostly just been been receiving messages from Meteor. Let's go ahead and respond to these messages and use Meteor's RPC capabilities. 

Let's say that Ada Lovelace is our personal favorite computer scientist, and we want to make sure she is always in the lead. So let's create a simple auto voter. Anytime we see someone other than Ada voted for we are going to give Ada a vote to keep her in the lead.

First let's handle added messages using pattern matching and find Ada in our lib/leader.ex

```elixir
defmodule Leader do
  use DDPHandler
  use GenServer

  def start_link do
    GenServer.start_link(__MODULE__, [], [])
  end

  def init([]) do
    {:ok, client_pid } = Philae.DDP.connect("ws://localhost:3000/websocket", __MODULE__, self)
    {collection, id} = Philae.DDP.subscribe(client_pid, "players")
    {:ok, %{client_pid: client_pid, subscription_id: id, collection: collection}}
  end

  def added(pid, message) do
    GenServer.call(pid, {:added, message})
  end


  def handle_call({:added, %{"fields" => %{"name" => "Ada Lovelace"}, "id" => id} = message}, _from, state) do
    Logger.info "Yeah! Ada is my favorite"
    Logger.info "In: " <> inspect(message)
    new_state = Map.put_new(state, :ada_id, id)
    {:reply, :ok, new_state}
  end

  def handle_call({:added, message}, _from, state) do
    Logger.info "In: " <> inspect(message)
    {:reply, :ok, state}
  end
end
```

OK, now when we restart our iex session we see an extra log message telling us Ada was added and that she is in fact our favorite. 

```
iex(1)> Leader.start_link

22:29:05.764 [info]  WS In: {"server_id":"0"}

22:29:05.778 [info]  Unhandled message received %{"server_id" => "0"}

22:29:05.778 [info]  WS Out: {"version":"1","support":["1","pre2","pre1"],"msg":"connect"}

22:29:05.779 [info]  WS In: {"msg":"connected","session":"tSTDHqJH3pA2Y9vm8"}

22:29:05.781 [info]  In: %{"msg" => "connected", "session" => "tSTDHqJH3pA2Y9vm8"}
{:ok, #PID<0.157.0>}

22:29:05.781 [info]  WS Out: {"name":"players","msg":"sub","id":"7875df79-b4d5-4d33-a8af-f8c45970d111"}

22:29:05.782 [info]  WS In: {"msg":"added","collection":"players","id":"yiPp9GZpNoKcd8hjY","fields":{"name":"Ada Lovelace","score":60}}

22:29:05.784 [info]  Yeah! Ada is my favorite
```

What else is going on here, though? If look at the first handle_call, we used pattern matching to match on Ada's name and capture her ID. Next we are creating a new GenServer state from the previous state and adding Ada's ID; then returning it as the next state for the GenServer process. This means that when we receive changed messages later we will be able to tell if the changes were to Ada's record or not.

Now let's do just that. Let's implement handle_calls for :changed messages and use pattern matching and a guard clause to tell when it is Ada's record that has changed.

```elixir
defmodule Leader do
  use DDPHandler
  use GenServer

  def start_link do
    GenServer.start_link(__MODULE__, [], [])
  end

  def init([]) do
    {:ok, client_pid } = Philae.DDP.connect("ws://localhost:3000/websocket", __MODULE__, self)
    {collection, id} = Philae.DDP.subscribe(client_pid, "players")
    {:ok, %{client_pid: client_pid, subscription_id: id, collection: collection}}
  end

  def added(pid, message) do
    GenServer.call(pid, {:added, message})
  end

  def changed(pid, message) do
    GenServer.call(pid, {:changed, message})
  end

  def handle_call({:added, %{"fields" => %{"name" => "Ada Lovelace"}, "id" => id} = message}, _from, state) do
    Logger.info "Yeah! Ada is my favorite"
    Logger.info "In: " <> inspect(message)
    new_state = Map.put_new(state, :ada_id, id)
    {:reply, :ok, new_state}
  end

  def handle_call({:added, message}, _from, state) do
    Logger.info "In: " <> inspect(message)
    {:reply, :ok, state}
  end

  def handle_call({:changed, %{"id" => id} = message}, _from, %{ada_id: ada_id} = state) when id == ada_id do
    Logger.info "YAH another vote for ADA!"
    {:reply, :ok, state}
  end

  def handle_call({:changed, message}, _from, state) do
    Logger.info "PlayerVoter recieved changed msg:" <> inspect message
    {:reply, :ok, state}
  end
end
```

Here we can see on line 35 we are matching on Ada's ID and the ID of the current record being passed from the Meteor collection. When they match our guard matches and our function is dispatched and we see a celebratory message in our iex session. If they do not match, we move on and the next function matches and simply logs the message out as normal.

```
]  WS In: {"msg":"changed","collection":"players","id":"yiPp9GZpNoKcd8hjY","fields":{"score":65}}

22:42:15.908 [info]  YAH another vote for ADA!

22:42:18.274 [info]  WS In: {"msg":"changed","collection":"players","id":"dQcPFWRjRKhfGXy48","fields":{"score":60}}

22:42:18.274 [info]  PlayerVoter recieved changed msg:%{"collection" => "players", "fields" => %{"score" => 60}, "id" => "dQcPFWRjRKhfGXy48", "msg" => "changed"}
```

Ok now lets get to the voter fraud ... 

Meteor has RPC-like functionality that can be exposed by defining Meteor.methods. Let's implement a voting method to call.
update your leadeboard/leader.js to look like this:

```javascript  
// Set up a collection to contain player information. On the server,
// it is backed by a MongoDB collection named "players".

Players = new Mongo.Collection("players");

if (Meteor.isClient) {
  Template.leaderboard.helpers({
    players: function () {
      return Players.find({}, { sort: { score: -1, name: 1 } });
    },
    selectedName: function () {
      var player = Players.findOne(Session.get("selectedPlayer"));
      return player && player.name;
    }
  });

  Template.leaderboard.events({
    'click .inc': function () {
      Players.update(Session.get("selectedPlayer"), {$inc: {score: 5}});
    }
  });

  Template.player.helpers({
    selected: function () {
      return Session.equals("selectedPlayer", this._id) ? "selected" : '';
    }
  });

  Template.player.events({
    'click': function () {
      Session.set("selectedPlayer", this._id);
    }
  });
  Meteor.subscribe("players");
}

// On server startup, create some players if the database is empty.
if (Meteor.isServer) {
  Meteor.startup(function () {
    if (Players.find().count() === 0) {
      var names = ["Ada Lovelace", "Grace Hopper", "Marie Curie",
                   "Carl Friedrich Gauss", "Nikola Tesla", "Claude Shannon"];
      _.each(names, function (name) {
        Players.insert({
          name: name,
          score: Math.floor(Random.fraction() * 10) * 5
        });
      });
    }
  });
  Meteor.methods({
      vote: function(playerName) {
        check(playerName, String);
        var player = Players.findOne({name: playerName});
        Players.update(player, {$inc: {score: 5}});
      },
  });
  Meteor.publish("players", function (){
    return Players.find();
  });
}
```

Here we added a function to our isServer section of our Meteor app in [Meteor.methods](http://docs.meteor.com/#/basic/Meteor-methods). Then find a player by their PlayerName and update their score by an increment of 5.

Now let's implement a Voter GenServer process to vote for Ada.

Create a new file called lib/voter.ex with these contents:

```elixir
require Logger

defmodule Voter do
  use GenServer

  def start_link(client_pid) do
    GenServer.start_link(__MODULE__, [client_pid], [])
  end

  def init(client_pid) do
    {:ok, client_pid}
  end

  def vote(pid, player_name) do
    GenServer.cast(pid, {:vote, player_name})
  end

  def handle_cast({:vote, player_name}, [client_pid] = state) do
    Logger.info "Voter is Voting for " <> inspect(player_name)
    Philae.DDP.method(client_pid, :vote, [player_name])
    {:noreply, [client_pid]}
  end
end
```


Let's start and link our new Voter process/GenServer to our leader in lib/leader.ex in our init function and add its pid to the Leader GenServer state.

```elixir
  def init([]) do
    {:ok, client_pid } = Philae.DDP.connect("ws://localhost:3000/websocket", __MODULE__, self)
    {collection, id} = Philae.DDP.subscribe(client_pid, "players")
    {:ok, voter_pid} = Voter.start_link(client_pid)
    {:ok, %{client_pid: client_pid, subscription_id: id, collection: collection, voter_pid: voter_pid}}
  end
```

Finally, we just need to change our non Ada changed handle_call function to call our Voters vote function:

```elixir
  def handle_call({:changed, message}, from, %{voter_pid: voter_pid} = state) do
    Logger.info "PlayerVoter recieved changed msg:" <> inspect message
    Logger.info "Voting for Ada!"
    Voter.vote(voter_pid, "Ada Lovelace")
    {:reply, :ok, state}
  end
```

So our final lib/leader.ex looks like this:

```elixir
defmodule Leader do
  use DDPHandler
  use GenServer

  def start_link do
    GenServer.start_link(__MODULE__, [], [])
  end

  def init([]) do
    {:ok, client_pid } = Philae.DDP.connect("ws://localhost:3000/websocket", __MODULE__, self)
    {collection, id} = Philae.DDP.subscribe(client_pid, "players")
    {:ok, voter_pid} = Voter.start_link(client_pid)
    {:ok, %{client_pid: client_pid, subscription_id: id, collection: collection, voter_pid: voter_pid}}
  end

  # Client API
  def added(pid, message) do
    GenServer.call(pid, {:added, message})
  end

  def changed(pid, message) do
    GenServer.call(pid, {:changed, message})
  end

  def vote_for_player(pid, player_name) do
    GenServer.call(pid, {:vote, player_name})
    {:ok, []}
  end

  # Server API

  def handle_call({:vote, player_name}, _from, %{client_pid: client_pid} = state) do
    Philae.DDP.method(client_pid, :vote, [player_name])
    {:reply, :ok, state}
  end

  def handle_call({:added, %{"fields" => %{"name" => "Ada Lovelace"}, "id" => id} = message}, _from, state) do
    Logger.info "Yeah! Ada is my favorite"
    Logger.info "In: " <> inspect(message)
    new_state = Map.put_new(state, :ada_id, id)
    {:reply, :ok, new_state}
  end

  def handle_call({:added, message}, _from, state) do
    Logger.info "In: " <> inspect(message)
    {:reply, :ok, state}
  end

  def handle_call({:changed, %{"id" => id} = message}, _from, %{ada_id: ada_id} = state) when id == ada_id do
    Logger.info "YAH another vote for ADA!"
    {:reply, :ok, state}
  end

  def handle_call({:changed, message}, from, %{voter_pid: voter_pid} = state) do
    Logger.info "PlayerVoter recieved changed msg:" <> inspect message
    Logger.info "Voting for Ada!"
    Voter.vote(voter_pid, "Ada Lovelace")
    {:reply, :ok, state}
  end
end
```

### Conclusion
Whenever we vote for someone now, we should see that Ada gets a vote almost immediately after. Pretty neat.

This is a limited example of using Philae and what you can do with Meteor and Elixir. Philae has a long way to go before it is ready for primetime. We are still working on it as a group at the [Cincinnati Elixir Meetup](http://www.meetup.com/Cincinnati-Elixir-Meetup) and plan to release it as a hex Package as soon as it gets a little further along. 
