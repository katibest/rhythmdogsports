---
post_author: Bill Barnett
categories:
- Development
tags: []
post_title: Configuring Ubuntu 10.04 for Ruby on Rails Development
publish_date: 2011-10-04T19:01:00.000+00:00
layout: post
current_gaslighter: true
feature_post_image: ''
post_images: []
slug: configuring-ubuntu-10-04-for-ruby-on-rails-development
permalink: "/blog/:slug"

---
While mentoring local high school students we needed to configure a number of
systems for [Rails](http://rubyonrails.org/) development.
[Ubuntu](http://www.ubuntu.com/) seemed to be the most reasonable choice of
OS. Below are the configuration steps we used broken into one-time setup,
repeatable demo app creation, and next steps.

### Motivation

This system is intended to be used by American high school students
with little or no programming experience and ZERO Linux system
administration experience. A seasoned developer will find this setup
too simplistic since many of the tools that a proficient Ruby developer
relies on but which might confuse a novice Rubyist have been omitted.

Here are some pain points encountered when novice developers where
initiated using a _"typical"_ Linux-based development system:

* Explaining the need for Bundler & RVM/rb-env drastically increased
  the learning curve.
* Editors like emacs and vi were too esoteric for novice developers.
* Choosing Heroku & PostgreSQL permitted us to simply _"gloss over"_
  the deployment process.

```sh
#!/bin/bash

# Update Ubuntu package information and install the minimum require software.
sudo apt-get -y update
sudo apt-get -y upgrade
sudo apt-get -y install build-essential curl git-core openssh-server
sudo apt-get -y install sqlite3 libsqlite3-dev postgresql postgresql-server-dev-8.4

# These are needed by the Redcar editor:
sudo apt-get -y install openjdk-7-jre
wget -O xulrunner.deb http://launchpadlibrarian.net/70321329/xulrunner-1.9.2_1.9.2.17%2Bbuild3%2Bnobinonly-0ubuntu1_amd64.deb
sudo dpkg -i xulrunner.deb

## These need to be installed before ruby.
sudo apt-get -y install libssl-dev libreadline6-dev libxml2-dev zlib1g zlib1g-dev libxss1

sudo apt-get -y install ruby1.9.1 ruby1.9.1-dev libyaml-dev
```

```sh
# Install Rubygems locally
mkdir ~/gems ~/src
cd ~/src
wget http://production.cf.rubygems.org/rubygems/rubygems-1.8.17.tgz
tar -xzvf rubygems-1.8.17.tgz
cd rubygems-1.8.17
ruby setup.rb --prefix=~/
ln -s ~/bin/gem1.9.1 ~/bin/gem

# Set environment variable in ~/.bashrc, add these lines at the bottom of the file:
export GEM_HOME=~/gems
export PATH=$HOME/bin:$GEM_HOME/bin:$PATH

# Then source the changed ~/.bashrc file:
. ~/.bashrc

# Install Rails
gem install rails

# Install Redcar
# ? gem install psych
gem install redcar

## Per: https://redcar.lighthouseapp.com/projects/25090/tickets/585-redcar-013-crashes-at-startup-on-ubuntu-1110
## Edit ~/gems/specifications/redcar-0.13.gemspec and replace this line:
##   s.date = %q{2012-01-21 00:00:00.000000000Z}
## With this:
##   s.date = %q{2012-01-21}

redcar install
```

```sh
# Create demo app using the Postgresql database and delaying the call to bundle install
rails new demo --database=postgresql --skip-bundle
cd demo                         # cd to the newly created Rails project root
# Add 'therubyracer' (a JavaScript runtime environment) to the Gemfile after: gem 'pg'
ruby -pi -e "puts %{gem 'therubyracer', :require => 'v8'} if $.==9" Gemfile
bundle install --path vendor    # run the `bundle install` that we delayed earlier
bundle exec rails server        # and start a local server

# open your browser and visit:
http://localhost:3000/
# or from another machine:
http://{IP-address-of-this-machine}:3000/
```

## Next steps

... (yes, this is the start of a syllabus or at least an agenda for Wednesday)

1. git
   1. edit config/database.yml
      * discuss trade-off between SQLite3 and PostgreSQL for local development
   1. create development and testing databases

      ```sh
      sudo su - postgres
      createuser -s web-apps -P # Enter any password you choose but be sure to remember it!
      createdb -O web-apps demo_development
      createdb -O web-apps demo_test
      exit
      ```

   1. restart server
1. create models, migrations, controllers, etc.
1. Testing!
   * mini::spec and mini::unit
   * RSpec and BDD v. TDD
1. "Heroku":http://heroku.com/
1. "Capistrano":http://capify.org/
1. "NewRelic RPM":https://rpm.newrelic.com/affiliates/AGILE210RFP/signup
