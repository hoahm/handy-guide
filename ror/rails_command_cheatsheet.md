Rails command cheatsheet

This cheatshet gives you a list of all common and useful commands with rails. To know more about rails command, click [here](http://guides.rubyonrails.org/command_line.html).

# rails new

Use for create new Rails application directory structure

    rails new <app_name>

By default, Rails using sqlite. To specify database using PostgreSQL or MySQL

    rails new <app_name> --database=postgresql
    rails new <app_name> --database=mysql

For MongoDB, [read here](https://gorails.com/guides/setting-up-rails-4-with-mongodb-and-mongoid)

# rails dbconsole

Open command line interface for interacting with database.

# rails generate

## Controllers

    rails generate controller home index  --no-helper --no-assets --no-controller-specs --no-view-specs

## Models

Use for migration. [Click here](http://edgeguides.rubyonrails.org/active_record_migrations.html) to know more.

### Generate inherit model

    rails generate model user email:string full_name:string dob:date

This command will generate a User model with three attributes: email, full_name, dob

    # /app/models/user.rb
    class User
    end

Here is the list of all available data types:

* integer
* primary_key
* decimal
* float
* boolean
* binary
* string
* text
* date
* time
* datetime
* timestamp

![](http://i.stack.imgur.com/Q0j0x.png)

![](http://i.stack.imgur.com/tuRiZ.png)

If you are using PostgreSQL, you can take advantages of these:

* [hstore](http://www.postgresql.org/docs/9.1/static/hstore.html)
* json
* [array](https://dockyard.com/blog/ruby/2012/09/18/rails-4-sneak-peek-postgresql-array-support)
* [cidr_address](http://www.postgresql.org/docs/current/static/datatype-net-types.html)
* ip_address
* mac_address

### Generate inherit model

    rails g model admin --parent user

This will generate

    # /app/models/admin.rb
    class Admin < User
    end

### Generate model scope

    rails g model admin/user

This feature allows to have separated namespaced models in rails code as in db schema

    # app/models/admin/user.rb
    module Admin
      def self.table_name_prefix
         'admin_'
      end
    end

### Generate index

    rails g model user email:index location_id:integer:index

### Generate uniq index:

    rails g model user pseudo:string:uniq

### Set limit for field of integer, string, text and binary fields:

    rails generate model user pseudo:string{30}

### Special syntax to generate decimal field with scale and precision:

    rails generate model product 'price:decimal{10,2}'

### You can combine any single curly brace option with the index options:

    rails generate model user username:string{30}:uniq

### Generate reference columns (fields which are used in rails as foreign keys):

    rails generate model photo album:references

### For polymorphic reference use this syntax:

    rails generate model product supplier:references{polymorphic}

### Polymorphic reference with indexes:

    rails generate model product supplier:references{polymorphic}:index

