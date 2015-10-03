This document will list some common and useful gems in Rails development.

 * [Authentication & Authorization](#authentication-and-authorization)
 * [Template Rendering](#template-rendering)
 * [Frontend](#frontend)
 * [API](#api)
 * [Paging](#paging)
 * [File upload](#file-upload)
 * [Background job](#background-job)
 * [Configuration](#configuration)
 * [3rd Party Services](#3rd-party-service)
 * [Development environment](#development-environment)
 * [Deployment](#deployment)
 * [Others](#others)

# Authentication & Authorization

### Devise

[Devise](https://github.com/plataformatec/devise) is a flexible authentication solution for Rails based on Warde

### Devise async

[devise-async](https://github.com/mhfs/devise-async) send Devise's emails in background. Supports Resque, Sidekiq, Delayed::Job and QueueClassic.

### Cancan

[CanCan](https://github.com/ryanb/cancan) is an authorization library for Ruby on Rails which restricts what resources a given user is allowed to access. All permissions are defined in a single location (the Ability class) and not duplicated across controllers, views, and database queries.

[Cancancan](https://github.com/CanCanCommunity/cancancan) is the continuation of CanCan

### Pundit

[pundit](https://github.com/elabs/pundit) Minimal authorization through OO design and pure Ruby classes

### Omniauth

[omniauth](https://github.com/intridea/omniauth) is a flexible authentication system utilizing Rack middleware.

[omniauth-oauth2](https://github.com/intridea/omniauth-oauth2) is an abstract 
OAuth2 strategy for OmniAuth.

[omniauth-facebook](https://github.com/mkdynamic/omniauth-facebook) is a Facebook OAuth2 Strategy for OmniAuth.

[omniauth-twitter](https://github.com/arunagw/omniauth-twitter) is an OmniAuth strategy for Twitter.

[omniauth-google-oauth2](https://github.com/zquestz/omniauth-google-oauth2) is an Oauth2 strategy for Google

[koala](https://github.com/arsduo/koala) is a lightweight, flexible library for Facebook with support for OAuth authentication, the Graph and REST APIs, realtime updates, and test users.

# Template Rendering

### HAML

[Haml-rails](https://github.com/indirect/haml-rails) provides Haml generators for Rails 4

### Slim

[lim-rails](https://github.com/slim-template/slim-rails) provides Slim generators for Rails 3 and 4

# Frontend

### Boootstrap SASS

[bootstrap-sass](https://github.com/twbs/bootstrap-sass) is a Sass-powered version of Bootstrap, ready to drop right into your Sass powered applications.

### Autoprefixer rails

[autoprefixer-rails](https://github.com/ai/autoprefixer-rails) is a tool to parse CSS and add vendor prefixes to CSS rules using values from the Can I Use. This gem provides Ruby and Ruby on Rails integration with this JavaScript tool.

### JQuery UI rails

[jquery-ui-rails](https://github.com/joliss/jquery-ui-rails) is a jQuery UI for the Rails asset pipeline.


### Font awesome

[font-awesome-rails](https://github.com/bokmann/font-awesome-rails) provides the Font-Awesome web fonts and stylesheets as a Rails engine for use with the asset pipeline.

### Select2 rails

[select2-rails](https://github.com/argerim/select2-rails) is a jQuery based replacement for select boxes. It supports searching, remote data sets, and infinite scrolling of results.

### JQuery Infinite pages

[jquery-infinite-pages](https://github.com/magoosh/jquery-infinite-pages) is a simple infinitely scrolling pages for jQuery, gemified for Rails.

### modernizr-rails

[modernizr-rails](https://github.com/russfrisch/modernizr-rails) is a Gem wrapper to include the Modernizr.js library via the Rails 3.1 asset pipeline.

### React Rails

[react-rails](https://github.com/reactjs/react-rails) is a Ruby gem for automatically transforming JSX and using React in Rails.

### momentjs-rails

[momentjs-rails](https://github.com/derekprior/momentjs-rails) The Moment.js JavaScript library ready to play with the Rails Asset Pipeline

### bower-rails

[bower-rails](https://github.com/rharriso/bower-rails/) Bower support for Rails projects. Dependency file is bower.json in Rails root dir or Bowerfile if you use DSL

### autonumeric-rail

[autonumeric-rail](https://github.com/randoum/autonumeric-rails) autoNumeric.js library with an ujs flavor, ready-to-use for rails.

### Breadscrumbs on Rails

[breadcrumbs_on_rails](https://github.com/weppos/breadcrumbs_on_rails) is a simple Ruby on Rails plugin for creating and managing a breadcrumb navigation.

# API

### Grape

[grape](https://github.com/ruby-grape/grape) is an opinionated micro-framework for creating REST-like APIs in Ruby

### Grape Entity

[grape-entity](https://github.com/ruby-grape/grape-entity) is an API focused facade that sits on top of an object model.

### Grape Apiary

[grape-apiary](https://github.com/connexio-labs/grape-apiary) used for generating an Apiary Blueprint (http://apiary.io) for your Grape API.

### Grape Swagger 

[grape-swagger](https://github.com/ruby-grape/grape-swagger) add swagger compliant documentation to your grape API

### Grape rails cache

[grape-rails-cache](https://github.com/monterail/grape-rails-cache) is a HTTP and server side cache integration for Grape and Rails. To use this gem, you have to install [redis-rails](https://github.com/redis-store/redis-rails) first.

# Paging

### Will paginate

[will_paginate](https://github.com/mislav/will_paginate) is a pagination library for Rails, Sinatra, Merb, DataMappe.

[will_paginate-bootstrap](https://github.com/bootstrap-ruby/will_paginate-bootstrap) integrates the Twitter Bootstrap pagination component with will_paginate.

### Kaminary

[kaminary](https://github.com/amatsuda/kaminari) is a Scope & Engine based, clean, powerful, customizable and sophisticated paginator for Rails 3 and 4.

[bootstrap-kaminari-views](https://github.com/matenia/bootstrap-kaminari-views) is a basic Gem for quick default inclusion of Kaminari theme compatible with Twitter Bootstrap 2.0 and Twitter Bootstrap 3.0

# File Upload

### Paperclip

[paperclip](https://github.com/thoughtbot/paperclip) is and easy file attachment management for ActiveRecord.

### Carrierwave

[carrierwave](https://github.com/carrierwaveuploader/carrierwave) is a classier solution for file uploads for Rails, Sinatra and other Ruby web frameworks.

### aws-sdk

[aws-sdk](https://github.com/aws/aws-sdk-ruby) The official AWS SDK for Ruby.

### s3_direct_upload

[s3_direct_upload](https://github.com/waynehoover/s3_direct_upload) Direct Upload to Amazon S3 With CORS

# Configuration

### Figaro

[figaro](https://github.com/laserlemon/figaro) is simple Rails app configuration

### Dotenv

[dotenv](https://github.com/bkeepers/dotenv) loads environment variables from `.env`.

# 3rd Party Service

## Exception Handling & Performance Monitoring

### HoneyBadger

[honeybadger](https://github.com/honeybadger-io/honeybadger-ruby) is a ruby gem for reporting errors to honeybadger.io

### Newrelic

[Newrelic](https://github.com/newrelic/rpm) is a New Relic RPM Ruby Agent.

Use [newrelic-grape](https://github.com/xinminlabs/newrelic-grape) for monitoring performance of Grape api.

## Push notification

### Parse ruby client

[parse-ruby-client](https://github.com/adelevie/parse-ruby-client) is a simple Ruby client for the parse.com REST API

## SMS & Phone Service

## Nexmo

[nexmo](https://github.com/timcraft/nexmo) used for sending sms globally.

### Twillio-ruby

[twillio-ruby](https://github.com/twilio/twilio-ruby) used for sending sms globally.

### hoiio

[hoiio](https://github.com/stvvan/hoiio-ruby) Hoiio SDK for Ruby

### Fibo SMS

[fibo](https://github.com/voanhduy1512/fibo) used for sending sms in Vietnam.

### intercom-rails

[intercom-rails](https://github.com/intercom/intercom-rails) Intercom is one place for every team in an internet business to communicate with customers, personally, at scale—on your website, inside web and mobile apps, and by email.

## Realtime

### Firebase

[firebase-ruby](https://github.com/oscardelben/firebase-ruby) is a Ruby wrapper for Firebase.

### Pusher

[pusher-http-ruby](https://github.com/pusher/pusher-http-ruby) is a Ruby server library for the Pusher API, used for realtime application.

## Payment

### paypal-sdk-core

[paypal-sdk-core](https://github.com/paypal/sdk-core-ruby) Core Library for PayPal Ruby SDKs

### 

# Background job

### Delayed job

[delayed_job](https://github.com/collectiveidea/delayed_job)

### Resque

[resque](https://github.com/resque/resque) is a Redis-backed Ruby library for creating background jobs, placing them on multiple queues, and processing them later. http://github.com/blog/542-introducing-resque

### Sidekiq

[sidekiq](https://github.com/mperham/sidekiq) is a simple, efficient background processing for Ruby

# Development and Testing

### Mailcacther

[mailcatcher](https://github.com/sj26/mailcatcher) catches mail and serves it through a dream.

### letter_opener

[letter_opener](https://github.com/ryanb/letter_opener) Preview mail in the browser instead of sending.

### Did you mean

[did_you_mean](https://github.com/yuki24/did_you_mean)

### rack-mini-profiler

[rack-mini-profiler](https://github.com/MiniProfiler/rack-mini-profiler) Middleware that displays speed badge for every html page. Designed to work both in production and in development.

### awesome_print
 
[awesome_print](https://github.com/michaeldv/awesome_print) Awesome Print is a Ruby library that pretty prints Ruby objects in full color exposing their internal structure with proper indentation. Rails ActiveRecord objects and usage within Rails templates are supported via included mixins.

### shog

[shog](https://github.com/phallguy/shog) Simple colored logging for rails 4 apps 

### timecop

[timecop](https://github.com/travisjeffery/timecop) A gem providing "time travel", "time freezing", and "time acceleration" capabilities, making it simple to test time-dependent code. It provides a unified method to mock Time.now, Date.today, and DateTime.now in a single call.

### simplecov

[simplecov](https://github.com/colszowka/simplecov) Code coverage for Ruby 1.9+ with a powerful configuration library and automatic merging of coverage across test suites

### webmock

[webmock](https://github.com/bblimke/webmock) Library for stubbing and setting expectations on HTTP requests in Ruby

### bullet

[bullet](https://github.com/flyerhzm/bullet) help to kill N+1 queries and unused eager loading.

### rerun

[rerun](https://github.com/alexch/rerun) Restarts an app when the filesystem changes. Uses growl and FSEventStream if on OS X.

### meta_request

Use [meta_request](https://github.com/dejan/rails_panel/tree/master/meta_request) and [](https://github.com/dejan/rails_panel) for supporting Rails development on Chrome.

### better_errors

[better_errors](https://github.com/charliesome/better_errors) Better error page for Rack apps

### brakeman

[brakeman](https://github.com/presidentbeef/brakeman) A static analysis security vulnerability scanner for Ruby on Rails applications 

# Deployment

### Mina

[mina](https://github.com/mina-deploy/mina) is a really fast deployer and server automation tool.

### Capistrano

[capistrano](https://github.com/capistrano/capistrano) is a remote multi-server automation tool.

# Others

### Enumerize

[enumerize](https://github.com/brainspec/enumerize) Enumerated attributes with I18n and ActiveRecord/Mongoid support.

### Simple form for

[simple_form](https://github.com/plataformatec/simple_form) Forms made easy for Rails! It's tied to a simple DSL, with no opinion on markup.

### Nested form for

[nested_form](https://github.com/ryanb/nested_form) is a Rails plugin to conveniently handle multiple models in a single form.

### country_select

[country_select](https://github.com/stefanpenner/country_select) Gemification of rails's country_select

### has_scope

[has_scope](https://github.com/plataformatec/has_scope) map incoming controller parameters to named scopes in your resources.

### Inherited resources

[inherited_resources](https://github.com/josevalim/inherited_resources) speeds up development by making your controllers inherit all restful actions so you just have to focus on what is important. It makes your controllers more powerful and cleaner at the same time.

### Geocoder

[geocoder](https://github.com/alexreisner/geocoder) is a complete Ruby geocoding solution.

### Pg_search

[pg_search](https://github.com/Casecommons/pg_search) builds ActiveRecord named scopes that take advantage of PostgreSQL's full text search.

### Friendly URL

[friendly_id](https://github.com/norman/friendly_id) is the “Swiss Army bulldozer” of slugging and permalink plugins for ActiveRecord. It allows you to create pretty URL’s and work with human-friendly strings as if they were numeric ids for ActiveRecord models.

### rails-settings-cached

[rails-settings-cached](https://github.com/huacnlee/rails-settings-cached) This is improved from rails-settings, added caching for all settings

Use it with [rails-settings-ui](https://github.com/accessd/rails-settings-ui) 
for manage settings in rails app.

### chartkick & groupdate

[chartkick](https://github.com/mher/chartkick.py) & [groupdate](https://github.com/ankane/groupdate) for creating beautiful Javascript charts with one line of Ruby.

### Audited

[audited](https://github.com/collectiveidea/audited) (previously acts_as_audited) is an ORM extension that logs all changes to your models. Audited also allows you to record who made those changes, save comments and associate models related to the changes.

### public_activity

[public_activity](https://github.com/chaps-io/public_activity) Easy activity tracking for models - similar to Github's Public Activity

### Savon

[savon](https://github.com/savonrb/savon) Heavy metal SOAP client

### whenever

[whenever](https://github.com/javan/whenever) is a Ruby gem that provides a clear syntax for writing and deploying cron jobs.

### wisper

[wisper](https://github.com/krisleech/wisper) A micro library providing Ruby objects with Publish-Subscribe capabilities

### barby

[barby](https://github.com/toretore/barby) The Ruby barcode generator

### apartment

[apartment](https://github.com/influitive/apartment) Database multi-tenancy for Rack (and Rails) applications

[apartment-sidekiq](https://github.com/influitive/apartment-sidekiq) Sidekiq support for the Apartment Gem
