---
layout: post
post_author: Zack Kayser
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'Elm, Elixir, and Phoenix: Reflecting on a Functional Full-Stack Project'
publish_date: 2018-02-26 20:40:00 +0000
feature_post_image: ''
post_images: []
slug: elm-elixir-and-phoenix-reflecting-on-a-functional-full-stack-project

---
A couple days ago, I wrapped up work on a side-project I started back in December 2016. It started out as a barebones server-side-rendered Phoenix app, and my only real goal at the time was to do a deep-dive into both Elixir and the Phoenix framework itself. The application is a classic Texas Hold ‘Em app, something I chose for a couple of reasons: 1) poker games like Hold ‘Em present excellent opportunities to take advantage of some of the most powerful features around concurrency and fault-tolerance made available by the OTP platform, and 2) the acceptance criteria were relatively well-defined and straightforward (or so I thought at the time… How hard could it be, right?)

I had a working application by March last year, albeit with quite a clunky UI. Most of the user-interaction functionality on the client was written in plain old JavaScript, a choice I made at the time thinking I would be perfectly content with the result. Just a splash of interactivity on the client where I needed it, and everything would be okay… Yeah... That actually didn't work out very well for me.

At any rate, I did have a real-time poker app that spun up 100 poker “room” processes that ran concurrently when the application started up. Each room took advantage of a range of OTP-related goodies: connections through Phoenix channels, running in `GenServer` (actually `GenStatem`) processes, and complete with supervisors for each room process. In three months, I went from not having ever worked on a real-time application to feeling like I could handle even the most daunting problems the domain could throw at me. Elixir does that. It’s empowering. OTP and Phoenix channels can give you superpowers.

Having taken my deep-dive on Elixir and feeling pretty good about what I learned, I shelved the project for a while. The lack of attention paid to the client-side code weighed on me, though. I knew it could be better, but I had trouble finding the motivation to pretty up the JavaScript code. I wasn't really sharing the app with anyone, and I didn't really see too much value in refactoring what was already there. Moving the code over to a front-end framework like React with a state management solution like Redux certainly would have been a big win, and I did consider rewriting the client code with React and Redux. For whatever reason, though, I couldn’t find the motivation to actually do it.

# Elm - Front and Center

Right around the time I stopped working on the project, I started hearing more and more about Elm. I began working through some simple tutorials, building out little widgets with it, and reading all I could about Elm. Since I had just immersed myself in Elixir, I had a newfound appreciation for functional languages and the functional mindset. But as I read more about Elm, I found other exciting aspects of the language:

- The Elm architecture encourages good design decisions, and more importantly, makes it very painful to make poor design decisions
- The tooling around the language is first-class (tools like `elm-package` are a breath of fresh air compared to npm)
- The compiler is phenomenal (no longer do you have to worry about cryptic compiler error messages - the Elm compiler tells you what went wrong, where, and gives you suggestions about how to fix it or points you to documentation that provides more context)
- A focus on the long-term evolution of the language as a platform for writing clean and reactive user interfaces

After a six-month hiatus from my poker project, I decided to return to it with the goal of rewriting the client-side entirely in Elm. This would be my first sizable Elm project, and I honestly wasn’t sure how to write an SPA in Elm. Luckily, Richard Feldman -- a big name in the Elm community and author of Elm in Action -- open-sourced an SPA example in Elm which I used as my starting point. You can find that project [here](https://github.com/rtfeldman/elm-spa-example).

Now, with the project fresh in mind, I want to share my experience - not only working with Elm, but my thoughts on how it works with Elixir and Phoenix on the back-end.

# Elm and Elixir - A Match Made in Functional Heaven

I personally felt that writing in the functional paradigm on both the client and the server was a big productivity win for me, cutting down on the degree of context-switching I had to do between front-end and back-end code. While you can certainly write good functional Javascript code, Elm enforces functional purity and immutability of data. It decouples data and behavior. You don’t have to worry as much about surprise mutations of state or spooky action at a distance, and that is true now for both your client code with Elm and your server code with Elixir.
One of the questions I've been asked about a fair amount as I progressed with the project is how to configure `Brunch` to serve an Elm app from Phoenix. Now, although the server app I wrote and the client are in two separate repos and are separate projects, I am also using Phoenix to serve the Elm app along with some other static assets. Figuring out the `brunch-config` is something I had to wrestle with up front, but overall it is not terribly complicated -- especially with the new directory structure in Phoenix 1.3. Here is an overview of what you need:

1. Pull in `elm-brunch` as a `devDependency` in your `package.json`
2. Update the list of `watched` directories inside of the `paths` object in `brunch-config.js` to include your Elm directory
2. In your `brunch-config.js`, modify the `plugins` object to make use of the aforementioned `elm-brunch` library and setup some project-specific configuration (I will share my configuration below)
3. You *might* need to manually compile your Elm app using `elm-make` once initially and copy the Javascript file output into your project (I had to do this at first until I upgraded to version `0.10.0` of `elm-brunch`)

As promised, here is a look at the relevant parts of my `brunch-config.js`:

```
paths: {
  watched: ["static", "css", "js", "vendor", "elm"],
  public: "../priv/static"
},

plugins: {
  babel: {
    ignore: [/vendor/]
  },
  elmBrunch: {
    mainModules: ["elm/src/Main.elm"],
    makeParameters: ["--debug"],
    outputFolder: "js/"
  },
}
```

Pretty straightforward config here. Behind the scenes, `elmBrunch` runs the `elm-make` command for you. I am specifying the relative path to my `Main` module (the entry point to my Elm app) from the `assets` directory, and `elm-make` uses this information to get the file, compile it to JS, and then output the result to the `outputFolder`. The `--debug` flag is passed as a parameter to `elm-make`, which gives you a nice built-in time-traveling debugger that comes with Elm. The debugger is another awesome feature of Elm and is great for development. Simply remove the `--debug` flag when deploying the application so end-users don't end up with their own debuggers on screen when you're in production.

There are some other options `elm-brunch` exposes to you, but I found the above settings to work best for my needs. You can, for example, also specify a name for the JavaScript file that gets output from the `elm-make` command (it defaults to the same name as your main Elm module, with a .js extension and lowercase filename, i.e. `Main.elm` -> `main.js`), and you can specify where the `elm-make` executable lives if you are doing something special and don't have the binary on your path.

With the config out of the way, I was free to get started working on my Elm app and dive into some real interop between Phoenix and Elm.

## Elm and Phoenix Channels

There are a couple of Elm libraries available for connecting to Phoenix channels, and they both deserve mention here. One, `elm-phoenix-socket` is available through the Elm Package Manager, and the other `elm-phoenix` is not. I went with `elm-phoenix` simply because I was able to get it working first and I found it easier to use. The downside, of course, is that it is not available through the Elm Package Manager, so you are on your own in terms of managing it as a dependency. The library is not available through the Elm Package Manager because it is what is known as an Effect Manager, which you can read a little more about [here](http://simonh1000.github.io/2017/05/effect-managers/). Essentially, Effect Managers are Elm modules that expand upon the Elm platform itself. The good news is that you don't have to worry about any of that. Just download the repo from [Github](https://github.com/saschatimme/elm-phoenix/blob/master/src/Phoenix.elm) and copy it into your `assets/elm` directory, or just clone it into that directory directly and you should be good to go.

There are a few groundwork-related items you need to have in place to set up a socket connection and connect to your channels from Elm. You will have to import the following modules from `elm-phoenix` to set everything up: `Phoenix`, `Phoenix.Socket`, `Phoenix.Channel`, and `Phoenix.Push`.

### 1. The `Phoenix.Socket` module

The `Phoenix.Socket` module exposes functions and types that let you configure and setup your socket connection. In my app, I created a `socket` function that takes in `Session` data (containing an authentication token for the user, if logged in) and a `String` that points to my socket endpoint url on the server. The function returns a `Socket Msg`. The pipe operator in Elm works great with the functions exposed by the modules from `elm-phoenix`. Here is a look at my socket function:

```
socket : Session -> String -> Socket Msg
socket session socketUrl =
  	let
        params =
            case session.player of
                Just player ->
                    let
                        token =
                            AuthToken.authTokenToString player.token
                    in
                    [ ( "guardian_token", token ) ]

                Nothing ->
                    []
    in
    Socket.init socketUrl
        |> Socket.withParams params
        |> Socket.onOpen SocketOpened
        |> Socket.onClose (\_ -> SocketClosed)
        |> Socket.onAbnormalClose (\_ -> SocketClosedAbnormally)
```

In the main body of the function here (the part below `in`), I'm declaring the behavior I want from my socket in a pipeline - from the initialization of the socket, to the parameters I'm passing along to the server, to the messages I want to receive when the socket connection opens, closes, or closes abnormally. The `Phoenix.Socket` module exposes other functions as well that let you further fine-tune what messages you want to receive and respond to on other socket-related events. If you are reading this and are unfamiliar with the Elm architecture, this excerpt might be slightly confusing to you. I will get into a (very) brief overview of the Elm architecture later in this post that will shed some more light on how this all fits together, but [here](https://guide.elm-lang.org/architecture/) is an awesome guide if you don't want to wait and you want to know all of the things right now.

### 2. The `Phoenix` module

I call the `socket` function above in my `subscriptions` function, which in turn is used in the `main` function -- the entry point for an Elm application. The Elm runtime steps up and handles the subscriptions for you, you simply need to tell it what you are interested in subscribing to. Here is a look at my subscription function:

```
subscriptions : Model -> Session -> Sub Msg
subscriptions model session =
    let
        phoenixSubscriptions =
            [ Phoenix.connect (socket session model.socketUrl) model.channelSubscriptions ]

        -- more subscriptions unrelated to the socket connection setup here
  	in
  	Sub.batch [ phoenixSubscriptions, someOtherSubscription ]
```

The `Phoenix` module from the `elm-phoenix` library exposes a `connect` function that takes your `Socket Msg` and a list of channel subscriptions (a `List (Channel Msg)` type), and then handles the dirty work of connecting to the server. Once connected, you start receiving the messages you set up in the `socket` function back from the runtime, and you can respond to them accordingly in your `update` function.

### 3. The `Phoenix.Channel` module

As you can see in the `subscriptions` function above, my model stores a list of channel subscriptions. I am joining two separate channels in the module these code examples are from, and I have a function for each channel that handles the logic of connecting to the channel on the server and declaring what messages and events I am interested in. Here is a look at the code for hooking up to my `playerChannel`, where I get information about the player (user) who is logged in:

```
playerChannel : Player -> Channel Msg
playerChannel player =
    Channel.init ("players:" ++ Player.usernameToString player.username)
        |> Channel.onJoin (\_ -> ConnectedToPlayerChannel)
        |> Channel.on "player" (\payload -> UpdatePlayer payload)
        |> Channel.on "attr_updated" (\message -> NewUpdateMessage message)
        |> Channel.on "error" (\error -> NewErrorMessage error)
        |> Channel.on "player_search_list" (\payload -> UpdateSearchList payload)
```

The function takes a `Player` record and returns a `Channel Msg`. The code looks very similar to the `socket` connection -- you initialize the channel with the `Channel.init` function, passing in a string that indicates the name of the channel you want to join. Then I specify that I want to receive a `ConnectedToPlayerChannel` message when the channel has been successfully joined, and I set up a number of custom event listeners using the `Channel.on` function. `Channel.on` takes two parameters: a string specifying the name of a message from the channel, and a function that takes a payload (of type `Json.Decode.Value`) that you wrap in a `Msg`. The `Msg` returned from the function gets picked up in your `update` function and you can respond to it accordingly. Generally this means decoding the payload and updating your model. That would look something like this (inside of your `update` function's `case` statement):

```
UpdatePlayer payload ->
  	case Decode.decodeValue Profile.decoder payload of
        Ok newProfile ->
            ( ( { model | profile = newProfile }, Cmd.none ), NoOp )

        Err error ->
            ( ( { model | errorMessages = error :: model.errorMessages } , Cmd.none ), NoOp )
```

### 4. The `Phoenix.Push` module

The last module to touch on from `elm-phoenix` is the `Phoenix.Push` module. This module lets you initiate `push` requests over the socket connection to the server. You can think of these as simple http requests, only instead of communicating over http, they communicate through a socket connection. You would use a `push` if you want to send a request to the server from your client-side code. The `Phoenix.Push` module also works well with the pipe operator and allows you to neatly compose functions together. I have a number of functions that create `push` requests in my code, and I will show one here that asks the server to create a new poker room:

```
createGamePush : Model -> Cmd Msg
createGamePush model =
    let
        playerName =
            getPlayerName model

        push =
            Push.init ("private_rooms:" ++ playerName) "create_room"
                |> Push.withPayload (encodeNewGame model.newGame)
                |> Push.onOk (\_ -> RoomCreated)
                |> Push.onError (\payload -> CreateRoomFailed payload)
    in
    Phoenix.push model.socketUrl push
```


The `Push` module follows suit with the `Socket` and `Channel` modules - you use the `Push.init` function to kick things off, then pipe the result through to specify the events and messages you are interested in. The `Push.init` function takes two parameters: 1) a string that represents the name of the channel on the server, and 2) the name of the message you are sending to the channel. You then pipe this result to the `Push.withPayload` function and pass in the encoded parameters you want to pass along to the server. You pipe that result to the `onOk` and `onError` functions where you specify the messages you want to receive for a successful response or a failed response. Finally, in the main body of the function, you reach for the `Phoenix` module again to call `Phoenix.push`, which takes the socket url endpoint as a string and the `Push Msg` you specified in the `let` block as parameters. The `Phoenix.push` function returns a `Cmd Msg`. If you are unfamiliar with the Elm architecture, `Cmd`s are a key part of the platform and are a specification of some task you want to happen in the real world. Once you specify a command, you can return it in your `update` function and the Elm runtime takes care of the dirty work. Here is a simplified example from my `update` function where I use the `createGamePush` function to kick off a `Cmd`:

```
let
    game =
        model.newGame

    newGame =
{ game | owner = Player.usernameToString model.player.username }

    newModel =
    	{ model | newGame = newGame }

in
( newModel, createGamePush model )
```

That was just a very brief whirlwind tour of hooking an Elm app up to Phoenix channels. There are some other cool features like `Phoenix.Presence` that I have not yet worked with from Elm, but after using `elm-phoenix` for a couple of months, I can say that the library has been great to work with. I would definitely recommend it if you are looking for an Elm client for Phoenix channels.

## A Brief Overview of the Elm Architecture

I would be remiss if I did not touch on the Elm Architecture, which has probably been the most appealing aspect of the language to me personally. It provides you a path to follow that guides you to a cohesive, straightforward design. One of the reasons for this is the language's reliance on immutable data structures and pure functions. Another part is that the Elm architecture provides you with some simple constructs that serve as a template and drive you towards an intuitive and scalable design. Let's look at the three core parts of the Elm Architecture:

1. Model - A record (records are similar to JavaScript objects, but are immutable) that holds the state of your application. This is like the store in a Redux application.
2. Update - A function that takes in a `msg` (more on this later) and your `Model`. You generally run a case statement on the `msg` where you can use pattern matching to pick up specific `msgs` and update your model accordingly. The parallel to this in Redux would be reducers. `msg`s roughly parallel actions in Redux, but there is more to the story that I'll touch on below.
3. View - A function that takes in your Model (and sometimes other parameters) and returns a `Html msg`. It does pretty much what it says: it describes your view. The `Html msg` part confused me a bit at first, but you can read this as: Html that emits messages of type `msg`.

In more sophisticated apps, you can add `subscriptions` to the list above, but the idea is basically the same as the view - you have subscriptions that emit messages of type `msg`, which you can pick up in the update function and use to update your model. There are also two tangentially-related concepts that are key to understanding the Elm architecture:

### Union Types
Union types, which you can read more about [here](https://guide.elm-lang.org/types/union_types.html), are one of the things that set Elm apart. The `Msg` type concept that appeared in most of the excerpts I showed aboveis an example of a union type. Union types allow you to specify a data structure that can hold a value that can be of different types, but only one of those types can be used at a given point. The most ubiquitous example from Elm itself is the `Maybe` union type. The `Maybe` type acts like optionals in other languages - it can either hold a single value of any type, or be empty. This is represented in `Maybe`'s definition: `type Maybe a = Just a | Nothing`. (The `a` is a type parameter, a stand-in for any type - the convention in Elm is to use lowercase letters/names for type parameters, like the `msg` I was describing above.) In Elm, union types generally demonstrate their power in case statements where you can use them with pattern matching:

```
aFunctionThatReturnsAnInteger : Maybe Int -> Int
aFunctionThatReturnsAnInteger maybeAnInteger =
case maybeAnInteger of
Just theInteger -> theInteger
Nothing -> 1
```

The first line of the code above is the type definition of the `aFunctionThatReturnsAnInteger` function, and, strictly speaking, is not needed. The compiler can figure out the type definition for you. The second line onward is the function definition, consisting of the function name followed by parameters. The `=` delineates the head of the function from the function body.

Union types are most important in terms of the Elm architecture in understanding the `update` function and one of its parameters - `Msg`. In a typical Elm app, you define your own union type, conventionally given the name `Msg`. Your `Msg` union type outlines all of the messages that you are interested in emitting and responding to from your module. Here is the `Msg` from my `Rooms.elm` module where I show a list of all `public` poker rooms running on the server:

```
type Msg
  = JoinedLobby
  | JoinFailed Decode.Value
  | UpdatePlayerCount Decode.Value
  | UpdateRooms Decode.Value
  | PaginationItemClicked String
  | SocketOpened
  | SocketClosed
  | SocketClosedAbnormally
```

I listen for all of these in my update function (Elm case statements must be exhaustive; use `_` as a catch-all, but be wary dear reader, this can be dangerous), and I use pattern matching to draw out values I'm interested in, like the `Decode.Value` that follows `UpdatePlayerCount`. The `Decode.Value` is the data sent back to me when the server broadcasts an update of the number of players who have joined a specific room. I use that data to update my `Model`, which updates the view accordingly. Other values I've defined here don't need any extra data. Receiving a `SocketOpened` value in my update function is enough to let me know what I need to do to update my model appropriately.

### `Cmd`
Elm enforces pure functions, so how do you do side-effecty things like make a network call? That is where `Cmd`s come in. A conventional `update` function generally returns a tuple of the form: `(Model, Cmd Msg)`. `Model` we already know - it is the (possibly updated) version of your model, but what is this `Cmd Msg`? Commands let you tell the Elm runtime to do something that has an effect on the outside world. They can send a `Msg` value back to your program you that you can respond to in your update function. You see this with Http requests, for example. In your update function, you might return something like `(model, Http.send MyMsg (Request.callApi model))`. The command here (the result of `Http.send`) is a description of what you would like the Elm runtime to do for you. You maintain functional purity while the runtime handles the dirty work for you. When it finishes, it will send the `MyMsg` value to your update function, which you can then use to update your model. This creates a common flow in Elm apps, something that follows along these lines:

1. You have a button in your view. Clicking on it emits a `SubmitForm` value.
2. You respond to the `SubmitForm` message in your update function by returning a `Cmd` in your update function without updating the model. The `Cmd` will send you another `Msg` with data.
3. You respond to the `Msg` you receive from the Cmd by using the data it wraps to update your model.

If you were wondering what to return from your `update` function when you don't want to do anything, by the way, I'm glad you asked. You can use `Cmd.none`. If you want to do more than one thing at a time, you can use `Cmd.batch (List (Cmd Msg))`. Pass a list of `Cmd Msg`s to the `Cmd.batch` function. `Cmd.none` and `Cmd.batch` also have twins in `Sub.none` and `Sub.batch` for subscriptions.


## Challenges and Roadblocks

I want to wrap up with some of the challenges I faced while I worked my way through Elm. I personally struggled with 1) how to organize my code, especially with larger modules, 2) figuring out how to make the UI more interactive, and 3) sharing code across modules. I am sure there are better ways to do these things than the way I handled them in my project, and I'd be super appreciative of any feedback. Let me just briefly touch on each one of these points.

1. Organizing code with larger modules - I have two modules that grew quite large by the end of the project: my `Page.Room` and `Page.Profile` modules. These represent the pages in the SPA for the actual poker room UI and for the player profile page, respectively. I took two separate approaches with each module.

 In the `Page.Room` module, I ended up breaking out my view, update, and socket config code into separate modules that I imported back into `Page.Room`. I also had to make a couple of modules specifically for some of the types I was using - one for the `Msg`, and another for other miscellaneous types.

 In the `Page.Profile` module, which I wrote after finishing the `Page.Room` module, I decided I would take a different approach and just keep everything in the same module regardless of how big it grew. It is now quite a massive file, but I actually feel that this approach worked better for me. I don't have to do weird things like import my `Msg` union type in a bunch of different files, and I don't have to navigate to other files to see what I'm actually doing in my update helper functions. Despite the size, I have things partitioned into segments, so my `Model` and type definitions are toward the top of the file, my view functions come next, then my update function, update helper functions, subscriptions, and finally a chunk of small utility helper functions. It is a huge file, but it actually does not take me too long to find where things are.

2. Making the UI more interactive - I have the feeling I found this difficult because I was coming to Elm with some baggage from JavaScript. In JavaScript, if I really wanted to make something move on the screen, I could just use a query selector to find an element in the DOM and manipulate it however I wanted. This is very difficult to do in Elm because of the focus on functional purity. Elm does have a library for lower level interactions with the DOM, but I couldn't figure out how to use it or even if it could do something like find an element and move it around. I have a function in my chat interface in the `Page.Room` module that scrolls the latest messages to the top of the chat element. To accomplish this, I decided to go with a `port` (ports are Elm's way of interop with JavaScript) that tells JavaScript to take over and handle the scrolling. Other interactive things I carry out using CSS transitions and animations. My struggle here is more with figuring out the right way to do address these kind of concerns in Elm.

3. Sharing code between modules - Where I really felt the pain of this one was in hooking up my socket to different modules. I connect to Phoenix channels in three different modules, and I ended up writing essentially the same boiler plate code to set up the socket connection in each module. Ideally, I would like to connect to the socket in the `Main` module and pass it down to pages where it was needed. The issue I ran into was that `Socket`s are parameterized with `Socket msg`. So, if I define a `Msg` in my `Main` module and get back a socket with type `Socket Msg`, the pages below that will be passed the same socket but want a socket instance parameterized with `Socket Room.Msg`, for example. I ran into similar issues when setting up the SPA itself with the discrepancy between `Html Msg`, `Html Room.Msg`, etc., but got around these issues using the `Html.map` function. The same is true for `Cmd Msg`s. I could map these from the `Main` module to wrap them in the appropriate type of `msg` for the specific page. I couldn't figure out how to do the same for sockets, though, even though I experimented with `Socket.map` for a while. I also have a feeling that there is a straightforward way around this that hasn't dawned on me yet. If I have a moment of insight, I will add an update here.

Overall, I absolutely loved working on this project - Elm has been a blast and I will certainly keep following the language and would love to use it on a real project at some point. Combining it with Elixir and Phoenix on the backend was a dream setup for me. I hope others feel the same way, and I'd love to see similar projects using the Functional Full Stack. If you want to check out the project, the Elm app is contained in [this](https://github.com/zkayser/pokerex_client) repo under `assets/elm`. The server-side code can be found [here](https://github.com/zkayser/poker_ex), and you can also visit the live app on Heroku [here](https://poker-ex.herokuapp.com/).