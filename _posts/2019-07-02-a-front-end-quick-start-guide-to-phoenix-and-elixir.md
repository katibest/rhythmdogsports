---
layout: post
post_author: Bailey Miller
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: A Front End Quick Start Guide to Phoenix and Elixir
publish_date: 2017-09-01 19:32:00 +0000
feature_post_image: ''
post_images: []
slug: a-front-end-quick-start-guide-to-phoenix-and-elixir

---
Elixir has taken our office by storm, with special thanks to a few Elixir evangelists in the office. Three out of eight of our current client projects at Gaslight use Elixir and Phoenix. We even have two developers [speaking at ElixirConf next week!](http://www.teamgaslight.com/elixir)

If you’re like me and work in the front end world, you probably haven’t worked with Elixir much or at all. You spend most of your time coding HTML, CSS, and/or JavaScript, and don’t know much about backend languages. If you’re like the designers at Gaslight, you may be used to maneuvering your way around Rails and JS based projects, but not Elixir since it is still early in the adoption curve. It’s difficult to find many resources for front end developers and designers on Elixir and Phoenix for this reason.

According to the [Elixir](https://elixir-lang.org/) website, Elixir is a “dynamic, functional language designed for building scalable and maintainable applications.” [Phoenix](http://phoenixframework.org/) is Elixir’s most popular web framework. If you’re used to Ruby, Phoenix is to Elixir as Rails is to Ruby. One more thing to know: Elixir is built on top of [Erlang](https://www.erlang.org/), which is a speedy language that’s been around for quite awhile (31 years!)


## Is This Post For You?

If you’re like me and just found yourself working on an Elixir project, unsure of how to set things up but hungry to learn, this post is for you! However, this post will not go into detail on actually making a new Elixir and Phoenix project from scratch. The information here will be most beneficial if you are working on an established Phoenix project. I am in no way an Elixir expert, so I am still trying to learn about the language myself. I hope this post will help you, as a front-ender, know what you need to know about Elixir so that you can understand how to set up your assets and maneuver your way around the project.

For this guide, it’ll be important for you to know that we’re using the following versions:

* Elixir version 1.5.1
* Phoenix version 1.3.0
* Erlang/OTP 20.0

_Note that file structure (especially around assets) has changed in a recent update to Elixir. Because of this, some of the details here might not apply if you’re using Phoenix version < 1.3._

We’re also using these JavaScript tools:

* **Yarn** for package management (instead of npm)
* **Brunch,** which comes with Phoenix by default for a build tool (instead of Webpack)


## Getting Started

### 1. Installing Elixir

Before you can do anything with Elixir, you have to install it on your computer globally. To do this, you’ll need a package manager. I use [Homebrew.](https://brew.sh/) Given you are using a Mac and have Homebrew, here’s what’ll you need to do to install Elixir:

```bash
# to make sure Homebrew is updated before diving in
brew update
# to install Elixir
brew install elixir
```

If you don’t have a Mac and don’t have Homebrew, view [these instructions.](https://elixir-lang.org/install.html)

And there you go. After that, you’ll have Elixir installed on your computer and you’ll never have to do it again! (Except you’ll need to update your Elixir version every once in awhile).

### 2. Installing Dependencies

```bash
mix deps.get
```

With this command, you’ll install all the Elixir and Phoenix dependencies in your project.

Note: when I did this, I had an error with something called “Hex.” When I ran `mix deps.get` my terminal window said:

```bash
Could not find Hex, which is needed to build dependency :phoenix
Shall I install Hex? (if running non-interactively, use "mix local.hex --force") [Yn]
```

I typed in `y` for yes. Then I ran `mix deps.get` once more and it worked. There’s a chance your terminal window might ask you to install other things with a similar kind of prompt. Just say `y` (unless it says `can I install a virus on your computer? [y/n]`.)

### 3. Create and Migrate your Database
```bash
mix ecto.create && mix ecto.migrate
```

Ecto is a database wrapper for Elixir. To get your database setup, you’ll need to run these commands.

When I did this, I ran into some gnarly postgres errors. I won’t go into detail for sanity’s sake, but for this part of the setup to work you’ll need postgres to be working. You can figure out how to get that setup [here.](https://www.codementor.io/devops/tutorial/getting-started-postgresql-server-mac-osx)

### 4. Install Node.js Dependencies


**If you’re using npm:**

```bash
cd assets && npm install
```

**If you’re using yarn:**

```bash
cd assets && yarn
```

Next you’ll need to install the dependencies that are handled with node. You’ll need to run a different command depending on whether you’re project is using npm or yarn. Npm is what your Elixir and Phoenix app will use by default, but we’re using yarn in the project I’m working on currently.

Another thing to note is that when installing dependencies, you’ll want to do it from the `assets` folder, not the root. (Hence the `cd assets` part of both of these commands.)


### 5. Starting your Server

```bash
mix phx.server
```

Run this command after setting up your dependencies. Or maybe your developer teammates (or you! I’m all for front-end empowerment) set up a bin/ command. The `/bin` folder is in place for different scripts. For example, we have a `bin/server` file that contains the `mix phx.server` command. So that way, if we type literally `bin/server` it will run whatever commands are in that file. Rad.

Now you can visit `localhost:4000` in your browser to check out your app!

So now once you’ve got your shiny app running you may be asking yourself “which things were a one time thing and which things will I need to do again?” Well, good question. Whenever you want to see your app running you will need to start your server with the above command, `mix phx.server`. The great thing about Elixir and Phoenix is the hot reloader that reloads the page whenever you make a change in code. That comes in handy!

Less frequently will you need to run these commands:

* `$ cd assets && yarn` or `$ cd assets && npm install` (depending on if you’re using yarn or npm) - You’ll run this when a node package is added to the package.json file in the assets folder
* `$ mix deps.get` - you’ll run this when a new dependency is added to the `mix.exs` file (which is where project dependencies are specified)
* `$ mix ecto.migrate` - you’ll run this when something new is added in your database

Often the terminal or view of the app will tell you when you need to do these things via an error, so keep an eye out and you’ll be golden.


## Setting Up Front End Assets
### File Organization

![file-tree](https://s3.amazonaws.com/gaslight-blog/a-front-end-quick-start-guide-to-elixir/blog-1.png)

This is what our file tree looks like. This will be different depending on what version of Elixir you’re using. Here’s a breakdown of the folders that will matter to a front end person.

* **`/assets`** - This is where your css, js, and “static” folders will live. The ‘/assets/static’ folder is where fonts and images will reside.
* **`/lib`** - This is where your templates will live (aka your HTML sprinkled with cool Phoenix logic). If you’ve used Rails you probably had `.erb` files. In Elixir, that extension is `.eex`. These `.eex` files exist in `lib/[project-name]_web/templates`. Within that, there should be a `layout` and `page` folder. In the `layout` folder there should be something called `app.html.eex` which exists as your most outermost template that most likely renders a page that exists in the `page` directory. This can be confusing if you’ve used Rails because templates exist in a `views` folder in Rails. But you may see in Elixir there’s a `views` folder that means something different.

### Installing Sass

The first thing I usually want to do once I’ve gotten a project running locally is set up sass so I can start setting up my css files. Here’s how you would do that in Elixir/Phoenix with Brunch.

**Installing sass with yarn:**

```bash
cd assets
yarn add sass-brunch
```

**Installing sass with npm:**

```bash
cd assets
npm install --save-dev sass-brunch
```

Once you’ve added sass to your package.json, you’ll need to add a few lines to your `assets/brunch-config.js` file. Here’s what I have in mine:

```
     stylesheets: {
      joinTo: "css/app.css",
      order: {
        after: ["css/app.scss"] // concat app.css last
      }
     },
```

Then after that, you’ll need to change the name of your main css file to have a sass extension, going from `assets/css/app.css` to `assets/css/app.scss`. From there, you should be good to start setting up your sass files!


### Setting Up Fonts
Web development can be a cruel world. A place where fonts never load correctly on the first go. Or second. Here is what our file tree looks like around our assets.

![assets-file-organization](
https://s3.amazonaws.com/gaslight-blog/a-front-end-quick-start-guide-to-elixir/blog-2.png)


The font files are in `assets/static/fonts` (within their respective font folders) and the css importing them is in `assets/css/base/_fonts.scss`. All of the partials including `_fonts.scss` are imported into `assets/css/app.scss`

Here is how we import our custom fonts in `_fonts.scss`:

```css
@font-face {
  font-family: 'Avenir';
  src: url('/fonts/avenir/AvenirLTStd-Black.otf');
  font-style: normal;
  font-weight: 800;
}
```

See the third line there? One stumbling block I had when configuring this was that I had copied my font sass file from a Rails project. The problem there was in Rails you often have to do weird things because of the asset pipeline like use `asset-url` instead of just `url`. In Elixir and Phoenix, you don’t need to go through the trauma drama of figuring out how the heck to work with the asset pipeline. So keep it simple with just `url` and it should work!

But...do you configure this route to go from the `_font.scss` file, `app.scss`, from the app route, from the assets folder, or the hidden `_/build` folder css files or wherever things end up? There are so many possibilities when you’re left to guess. Well, the answer is none of those!

Static assets like fonts and images within `assets/static` are copied to `priv/static`. So then if you have the default configuration of Brunch, everything in `priv/static` is served from `/`. So instead of referring to the example font above with the url `assets/static/fonts/avenir/AvenirLTStd-Black.otf` (which is the full path from the application’s root),you would leave off `assets/static` and just use `/fonts/avenir/AvenirLTStd-Black.otf` like I did in the above example. [This forum](https://elixirforum.com/t/how-to-serve-an-img-with-phoenix/2036/2) helped me figure that out.

Static urls should also work the same with images. If you want to render an image in an .eex template, here is what you would do if the image was located in `assets/static/images/image.jpg`:

```
 <img src="<%= static_path(@conn, "/images/image.jpg") %>">
```

## Glossary
Now that you’ve done _tons_ of great work setting up your Elixir and Phoenix project on your machine, you probably still are confused by all the terms being thrown around. Me too.

### Elixir
A dynamic, functional language designed for building scalable and maintainable applications.
### Phoenix
A web framework for Elixir.
### Erlang
The speedy and established language that Elixir was built on top of.
### OTP
Stands for “Open Telecom Platform”. A collection of useful middleware, libraries, and tools written in Erlang.
### Mix
Makes it so Elixir and Phoenix code can be sorted into project files. If you’re used to Ruby, Mix is like Bundler and Rake combined.
### Ecto
A database wrapper for Elixir. If you’re used to Ruby, Ecto is like Active Record.
### Hex
A package manager that comes with Elixir. It was built for all of Erlang. If you’re used to Ruby, Hex is similar to RubyGems.
### Brunch
A build tool, an alternative to Webpack, and comes with Elixir by default.
### Yarn
A Node package manager. An alternative to npm.

## Conclusion

So there you have it. I hope this helped you, as a front-end person, to get started on your Phoenix project. I hope to see more documentation for designers, front-end developers, and more out there for Elixir.
