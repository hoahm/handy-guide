## 1. Initialize project

    rails new <project-name> --database=postgresql

## 2. Add rspec & some useful gems for testing, development environment

Open Gemfile, add the following gems. [click here](helpful_gems.md) for more gems.

    group :development, :test do
      # Call 'byebug' anywhere in the code to stop execution and get a debugger console
      gem 'byebug'

      gem 'faker'
      gem 'factory_girl_rails'
      gem 'database_cleaner'

      gem 'rspec-rails'

      gem 'airborne'
      gem 'shoulda-matchers'
      gem 'shoulda-callback-matchers', '~> 1.0'
      gem 'webmock'

      gem 'guard'
      gem 'guard-rails'
      gem 'guard-bundler'
      gem 'guard-rubocop'
      gem 'guard-rspec'

      gem 'spring-commands-rspec'
      gem 'did_you_mean'
      gem 'rubocop', require: false
      gem 'pry-rails'
      gem 'shog'
      gem 'better_errors'
    end

    group :development do
      # Access an IRB console on exception pages or by using <%= console %> in views
      gem 'web-console', '~> 2.0'

      # Spring speeds up development by keeping your application running in the background. Read more: https://github.com/rails/spring
      gem 'spring'

      gem 'binding_of_caller'
      gem 'quiet_assets'
      gem 'meta_request'
      gem 'guard-livereload', require: false
      gem 'letter_opener'
      gem 'rerun'
    end

## 3. Integrate Bootstrap template and some frontend libraries

    ##
    # ------------------------------
    # FRONT END
    # ------------------------------
    # template generator
    gem 'slim-rails'

    # twitter bootstrap
    gem 'bootstrap-sass'

    # Jquery UI
    gem 'jquery-ui-rails'

    # auto prefixer
    gem 'autoprefixer-rails'

    # bower dependencies manager for rails
    gem 'bower-rails'

    # modernizr
    gem 'modernizr-rails', '~> 2.7.1'

    # font awesome
    gem 'font-awesome-rails'

    # Noty JS library
    gem 'noty-rails'

## 4. Layout template

## 5. Design ERD 

Design ERD based on requirements.

## 6. Implement

For best practice, we need to implement API first.

### 6.1 Generate model

    rails g model <model-name> <attribute>:<type>

### 6.2 Implement API

Implement API using [grape](https://github.com/ruby-grape/grape) and [rape entity](https://github.com/ruby-grape/grape-entity).

    ##
    # ------------------------------
    # API
    # ------------------------------
    gem 'grape'
    gem 'grape-entity'
    gem 'grape-apiary'
    gem 'grape-swagger'
    gem 'grape-rails-cache'

### 6.3 Generate controllers and implement CRUD for admin.

Click here for example [Gemfile](Gemfile)
