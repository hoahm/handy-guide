This guide show you how to setup development environment for Ruby on Rails (MacOS X only). 

# Content

 1. [Prerequisites](#prerequisites)
 2. [Sublime Text](#sublime-text)
 3. [Configure GIT](#configure-git)
 4. [Generate SSH key](#generate-ssh-key)
 5. [Ruby, Rails and all dependencies](#ruby-rails-adn-all-dependencies)
 6. [PostgreSQL](#postgresql)
 7. [MySQL](#mysql)
 8. [Redis](#redis)
 9. [Elastic Search](#elastic-search)
 10. [Memcached](#memcached)
 11. [References](#references)


# Prerequisites

You have to have [Xcode](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) installed, and proceeded with the following commands

    sudo xcodebuild -license
    sudo xcode-select --install

Install homebrew

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

# Sublime Text

    Download and install [ublime Text 3](http://www.sublimetext.com/3) and [ackage Control](https://packagecontrol.io/installation).

Run this command to create alias

    ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/subl

## Packages

Then, install the following packages: 
 * DocBlockr
 * Emmet
 * SublimeLinter
 * BracketHighligter
 * Rspec
 * Guard
 * SideBarEnhancements
 * Haml
 * SCSS Snippets
 * SASS Snippets
 * Coffeescript
 * Solarized Color Scheme
 * Autocomplete
 * Color Highlighter
 * IcedCoffeeScript
 * RuboCop
 * Ruby Slim

## Customize SublimeText

Go to Sublime Text > Preferences > Settings (User) and paste the following:

    {
      "auto_complete": true,
      "auto_complete_commit_on_tab": true,
      "copy_with_empty_selection": true,
      "default_initial": "~/projects",
      "ensure_newline_at_eof_on_save": true,
      "font_size": 12,
      "ignored_packages":
      [
        "Markdown",
        "Vintage"
      ],
      "index_files": true,
      "rulers":
      [
        100
      ],
      "tab_size": 2,
      "translate_tabs_to_spaces": true,
      "trim_trailing_white_space_on_save": true,
      "word_separators": "./\\()\"'-:,.;<>~@#$%^&*|+=[]{}`~"
    }

# Configure GIT

    git config --global color.ui true
    git config --global user.name "YOUR NAME"
    git config --global user.email "YOUR@EMAIL.com"

# Generate SSH key

    ssh-keygen -t rsa -C "YOUR@EMAIL.com"

# Ruby, Rails and all dependencies

Install RVM with stable Ruby

    curl -sSL https://get.rvm.io | bash -s stable --ruby

Verify install
  
    rvm --version

Run the following command to not install gem document

    echo "gem: --no-ri --no-rdoc" > ~/.gemrc

Install bundler, rails gem

    gem install bundler
    gem install rails

Install a specific Ruby version

    rvm install 2.2.3

List all ruby versions
  
    rvm list

Use a specific Ruby version

    rvm use 2.2.3 --default        

# PostgreSQL

    brew update
    brew install postgres
    postgres -D /usr/local/var/postgres

To start PostgreSQL at startup

    cp /usr/local/Cellar/postgresql/9.0.1/org.postgresql.postgres.plist ~/Library/LaunchAgents
    launchctl load -w ~/Library/LaunchAgents/org.postgresql.postgres.plist

To create a new database

    psql postgres
    postgres=# CREATE ROLE <role_name> WITH LOGIN PASSWORD '<password>';
    postgres=# CREATE DATABASE <database_name> OWNER <role_name>;

More useful commands

    postgres=# ALTER ROLE <role_name> WITH CREATEDB;
    postgres=# ALTER ROLE <role_name> PASSWORD '<new_password>';
    postgres=# GRANT ALL PRIVILEGES ON DATABASE <database_name> TO <role_name>;
    postgres=# ALTER USER <role_name> WITH SUPERUSER;

To list all databases

    postgres=# \l

To list all schemas

    postgres=# \dn

To list all tables

    postgres=# \dt

To view table detail

    postgres=# \d+ <table_name>

To add extension to database name (for example postgis)

    psql database_name
    database_name=# CREATE EXTENSION IF NOT EXISTS postgis;

# MySQL

Run these commands and follow the instruction to install MySQL

    brew install mysql
    mysql_secure_installation

To have launchd start mysql at startup (not recommend)
    
    ln -sfv /usr/local/opt/mysql/*plist ~/Library/LaunchAgents

Then to load mysql now

    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist

# Redis

    brew install redis

To start Redis at startup (not recommend)

    ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents

Run redis with configuration file

    redis-server /usr/local/etc/redis.conf

# Elastic Search

    brew install elasticsearch 

Verify if install successfully

    brew info elasticsearch

To have launchd start elasticsearch at login:

    ln -sfv /usr/local/opt/elasticsearch/*.plist ~/Library/LaunchAgents

Then to load elasticsearch now:

    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.elasticsearch.plist

Or, if you don't want/need launchctl, you can just run:

    elasticsearch --config=/usr/local/opt/elasticsearch/config/elasticsearch.yml

# Memcache

    brew install memcached

Start memcached

    memcached -vv

To have launchd start memcached at login:

    ln -sfv /usr/local/opt/memcached/*.plist ~/Library/LaunchAgents

Then to load memcached now:

    launchctl load ~/Library/LaunchAgents/homebrew.mxcl.memcached.plist

Or, if you don't want/need launchctl, you can just run:

    /usr/local/opt/memcached/bin/memcached

# References

* [RVM instructions](https://rvm.io/rvm/install)
* [Install PostgreSQL via homebrew](http://exponential.io/blog/2015/02/21/install-postgresql-on-mac-os-x-via-brew/)
* [Install Redis via homebrew](https://medium.com/@petehouston/install-and-config-redis-on-mac-os-x-via-homebrew-eb8df9a4f298)
* [Setup RoR on MacOS 10.10 Yosemite](https://gorails.com/setup/osx/10.10-yosemite)
* [Getting started with Elastic Search](http://red-badger.com/blog/2013/11/08/getting-started-with-elasticsearch/)
