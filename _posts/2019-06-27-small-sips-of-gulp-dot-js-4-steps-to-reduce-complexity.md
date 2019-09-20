---
layout: post
post_author: Jim Anders
current_gaslighter: false
categories:
- Development
tags: []
permalink: "/blog/:slug"
post_title: 'Small Sips of Gulp.js: 4 Steps to Reduce Complexity'
publish_date: 2014-08-26T13:26:00.000+00:00
feature_post_image: "/uploads/Gulp.jpg"
post_images: []
slug: small-sips-of-gulp-dot-js-4-steps-to-reduce-complexity

---
Over the past couple months, I've been writing a lot of [JavaScript](http://teamgaslight.com/blog/search?utf8=%E2%9C%93&q=javascript) and taken a much deeper dive into the node.js world, more specifically into Gulp.js
as a build system for my client side work. One thing I've noticed when
looking at other gulpfiles is that they have a tendency to be
very verbose and not very DRY.

As the line count of my own gulpfile began to push beyond the 300
mark, I started to look for patterns and ways to DRY up the code. Here are four things I've found useful:

## 1. Eliminate Unnecessary Plugins The first obvious thing to me was the nasty mess of 25 lines of require statements at the top of the file. My first step was to evaluate each plugin to determine if we actually needed it.

Why load a plugin when using `child_process.exec()`
will accomplish the same task? Do we really need a separate file concatenator
when all we are using it for is to concatenate our JavaScript? And Uglifier will
handle that for us? Eliminating unnecessary plugins saved a few lines.

## 2. Sidestep (Most) Require Lines
The next step actually required installing a new plugin, but saved me all but
three of the require lines. [gulp-load-plugins](https://www.npmjs.org/package/gulp-load-plugins)
is a _fantastic_ little plugin that will take the liberty to load all plugins
in `node_modules` whose name begins with `gulp-`. You only need to instantiate
the plugin and the result is a POJO that you can stuff into a variable and pass
around.

    var $ = require('gulp-load-plugins')();

Inside of a gulp task you would call your plugin as normal, only now you
access it through the object you created.

    gulp.task('sass', ['clean:css'], function() {
      return gulp.src('path/to/sass')
        .pipe($.rubySass())
        .pipe(gulp.dest('dist/css')
    });

This cleanly reduced 20 lines of plugin require to one line.

## 3. Tap the Power of Node.js Modules
Step two helped reduce the complexity in our gulpfile, but there is still the issue of the
sheer file size. I find any file I have to scroll in to be too long.
By the time I get to the bottom, I've forgotten what's at the top.
And in the case of Gulp, I was losing track of which tasks were and weren't in
the system.

For this refactoring, I turned to the node.js module system. I'm not a node.js
expert by any means, but the documentation available on the module system is
decent, and there are plenty of tutorials on using it. My recommendation for
breaking up your Gulp file is to start small. Pull out one task at a time and
ensure that everything still runs as intended. During this
step in my own project, I discovered several redundant or similar tasks and was able to make them more reusuable with a few small changes.

Taking our Sass task from above and moving it into its own module could look
something like this:

    module.exports = function(gulp, $) {
      gulp.task('sass', ['clean:css'], function() {
        return gulp.src('path/to/sass')
          .pipe($.rubySass())
          .pipe(gulp.dest('dist/css'))
      });
    }

Extracting all of our tasks out into individual modules leaves us with a gulpfile
of nothing but require statements and small, single responsibility modules
that are clearly defined.

## 4. Weed Out Duplications
While the refactoring we just did was extremely helpful in
improving the readability of our build system, I found it introduced a lot of duplication. We had several command line arguments and different internal paths
that were being defined in many of our tasks.

To reduce this duplication, the power of POJOs and the node.js module system came into play again. We collected all of our options at the top of our
gulpfile then passed them into the module functions of each task that required
them. This step gave us something similar to this:

    var args = require('yargs').argv;
    var opts = {
      sync: args.sync,
      p: args.p,
      n: args.n
    }

Our Sass task could then use our opts object.

    module.exports = function(gulp, opts, $) {
      gulp.task('sass', ['clean:css'], function() {
        return gulp.src('path/to/sass')
          .pipe($.rubySass())
          .pipe(opts.p ? gulp.dest('dist/css') : gulp.dest('.tmp/css'))
      });
    }

All of our refactoring would leave our `gulpfile.js` like this:

    var args = require('yargs').argv,
        gulp = require('gulp'),
        $    = require('gulp-load-plugins')(),
        opts = {
          sync: args.sync,
          p: args.p,
          n: args.n
        }
    
    require('./tasks/sass')(gulp, opts, $);
    require('./tasks/scripts')(gulp, opts, $);
    require('./tasks/default')(gulp);

In just a few short steps, we have reduced the overall complexity and readability
of our `gulpfile.js`. We've extracted our tasks into small reusable modules, each
designed with a single responsibility. And reduced repeated code by extracting
out a simple POJO to hold our build system state.