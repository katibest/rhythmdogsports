---
layout: post
post_author: Zack Kayser
current_gaslighter: true
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'A Starter Project with Goodies: Phoenix 1.4, Elm 0.19, and Parcel'
publish_date: 2018-12-03 16:17:00 +0000
feature_post_image: "/uploads/phoenix_elm.png"
post_images: []
slug: a-starter-project-with-goodies-phoenix-1-dot-4-elm-0-dot-19-and-parcel

---
# A Starter Project with Goodies: Phoenix 1.4, Elm 0.19, and Parcel


There has been a lot of excitement in the world of functional web development over the past couple months: the release of Elm 0.19 in late August and the recent release of Phoenix 1.4 with shiny new bells and whistles like Phoenix LiveView.


With all of these new toys coming down the pipeline, it is a good time to set up a template starter app so we can dig in and get right to work without having to worry too much about project setup and configuration. To get us started, I created a minimal starter template that serves Elm assets from an Elixir backend with Phoenix 1.4. Instead of using webpack (now the default asset bundler in Phoenix 1.4), I opted for [Parcel](https://parceljs.org/) -- a tool that promises to be "blazing fast" with "zero configuration". It certainly has not let me down so far!


## Setting Up the Frontend


We can pull in all we need to build an Elm application and have Parcel bundle our Elm files in watch mode during development by making some slight modifications to the default `package.json` file created by running `mix phx.new` (which you should find in `your_app/assets/package.json`). 


```json
{
  "repository": {},
  "license": "MIT",
  "scripts": {
    "deploy": "parcel build ./js/app.js --out-dir priv/static/js",
    "watch": "parcel watch ./js/main.js --out-dir priv/static/js"
  },
  "dependencies": {
    "parcel-bundler": "^1.10.3",
    "phoenix": "file:../deps/phoenix",
    "phoenix_html": "file:../deps/phoenix_html",
    "growl": ">=1.10.0"
  },
  "devDependencies": {
    "node-elm-compiler": "^5.0.1"
  }
}
```


There are a couple changes to note here that differ from the defaults. First, we need to change the `scripts` object to use Parcel to build and watch our frontend code. In the `watch` script, we are simply telling Parcel to watch for changes in `js/main.js`, which is the output of compiling our Elm application. 


Next, we need to make sure to bring in `parcel-bundler` as a dependency, and we also specify the `node-elm-compiler` as a dev dependency. 


The next bit of configuration comes on the Elixir side in `config/dev.exs`. Here, we just need to change the watchers so Phoenix can hot reload any changes in our frontend code:


```elixir
config :phoenix_elm_template, PhoenixElmTemplateWeb.Endpoint,
  http: [port: 4000],
  debug_errors: true,
  code_reloader: true,
  check_origin: false,
  watchers: [
    node: [
      "./assets/node_modules/parcel-bundler/bin/cli.js",
      "watch",
      "./assets/js/app.js",
      "--out-dir",
      "priv/static/js"
    ]
  ]
```


Finally, to setup an Elm app, cd into the `assets` directory and run `elm init` if you have the `elm` binary installed globally (otherwise, you can specify `elm: 0.19` as a dependency in `package.json`, run `yarn` or `npm install`, and then point to the `elm` binary in your `node_modules` to initiate the project). This will create an `elm.json` file and a `src` directory where you can put your `.elm` files. We'll make a `Main.elm` file that will contain a trivial Elm app that you can build from.


```elm
module Main exposing (..)


import Browser
import Html exposing (..)
import Html.Events exposing (onClick)
import Html.Attributes exposing (class)


main = 
  Browser.sandbox { init = 0, update = update, view = view }


type Msg = Increment | Decrement


type alias Model = Int


update : Msg -> Model -> Model
update msg model =
  case msg of
    Increment -> model + 1
    Decrement -> model - 1


view : Model -> Html Msg
view model =
  div [ class "elm-container" ]
    [ button [ onClick Decrement, class "button" ] [ text "-" ]
    , div [] [ text (String.fromInt model) ]
    , button [ onClick Increment, class "button" ] [ text "+" ] 
    , div [] [ text "And here is some more stuff from Elm!"]
    ]
```


Now to serve the app, simply run `mix phx.server` and go to `http://localhost:4000` and voila! To see the live reloading in action, go ahead and make some changes to the `Main.elm` file with your server running. You should see the output from Parcel in the terminal with something like "Built in 355ms". Lightning fast hot reloads with Elm! You should also see any changes you made reflected immediately in your browser.


To see the code for the entire starter app, you can check it out or fork it from my Github [here](https://github.com/zkayser/phoenix-elm-template).