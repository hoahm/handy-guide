This guide will show you how to setup (Ubuntu 14.04) server using Nginx and Phusion Passenger for serving both Rails application and PHP application.

 * [Add swap](#swap)
 * [Config NTP](#config-ntp)
 * [User and permission](#user-and-permission)
 * [Install PostgreSQL](#install-postgresql)
 * [Install Ruby and all dependencies](#install-ruby-and-all-dependencies)
 * [Install Nginx with Passenger](#install-nginx-with-passenger)
 * [Install NodeJS](#install-nodejs)
 * [Install Redis](#install-redis)
 * [Install PHP](#install-php)
 * [Install Monit](#install-monit)

# Swap

Add swap file

    sudo fallocate -l 4G /swapfile
    sudo chmod 600 /swapfile
    sudo mkswap /swapfile
    sudo swapon /swapfile

Make the swap file pernament

    sudo nano /etc/fstab
    /swapfile   none    swap    sw    0   0

# Config NTP

    sudo dpkg-reconfigure tzdata
    sudo apt-get install ntp
    sudo ntpdate ntp.ubuntu.com
    # sudo ntpdate pool.ntp.org
    sudo ntpd -gq
    sudo service ntp start

    sudo timedatectl set-timezone Asia/Ho_Chi_Minh

List all timezones
    
    timedatectl list-timezones
    ntpq -p

# User and permission

Create new user

    useradd -d /home/webapp -m -s /bin/bash webapp
    passwd webapp

Add user webapp to sudoers

    sudo usermod -aG sudo webapp

    visudo

    webapp ALL=(ALL:ALL) NOPASSWD: ALL

Generate SSH key

    ssh-keygen -t rsa -C "your_email@example.com"

Add SSH key 

    cat ~/.ssh/id_rsa.pub | ssh root@[your server's address] "cd /home/webapp/.ssh && cat >> /home/webapp/.ssh/authorized_keys"

or

    scp ~/.ssh/id_rsa.pub root@[your server's address]:~/.ssh/authorized_keys

Change to bash shell if can not press tab for auto-complete the commands

    sudo chsh -s /bin/bash webapp

# Install PostgreSQL

    sudo apt-get install postgresql postgresql-contrib libpq-dev

Create PostgreSQL's user

    sudo su - postgres
    createuser --pwprompt admin
    exit

Create Role

    sudo -u postgres psql postgres
    postgres=# CREATE ROLE <role_name> WITH LOGIN PASSWORD '<password>';
    postgres=# CREATE DATABASE <database_name> OWNER <role_name>;

More useful commands

    postgres=# ALTER ROLE <role_name> WITH CREATEDB;
    postgres=# ALTER ROLE <role_name> PASSWORD '<new_password>';
    postgres=# GRANT ALL PRIVILEGES ON DATABASE <database_name> TO <role_name>;
    postgres=# ALTER USER <role_name> WITH SUPERUSER;

Import db from file

    psql -h hostname -d databasename -U username -f file.sql

# Install Ruby and all dependencies

Install dependencies

    sudo apt-get update
    sudo apt-get install -y git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev libgdbm-dev libncurses5-dev automake libtool bison libffi-dev libpq-dev

Install imagemagick for paperclip

    apt-get install -y imagemagick

Install rbenv and ruby-build

    cd ~
    git clone git://github.com/sstephenson/rbenv.git .rbenv
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
    echo 'eval "$(rbenv init -)"' >> ~/.bashrc
    exec $SHELL

    git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
    exec $SHELL

    git clone https://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash

    rbenv install 2.2.3
    rbenv global 2.2.3
    ruby -v

Do not install the gems' document

    echo "gem: --no-ri --no-rdoc" > ~/.gemrc

Install Bundle and Rails

    gem install bundler
    gem install rails

# Install Nginx with Passenger

    gem install passenger 
    sudo /home/webapp/.rbenv/shims/passenger-install-nginx-module

Add Nginx startup script by downloading nginx startup script

    wget -O init-deb.sh https://www.linode.com/docs/assets/660-init-deb.sh

Move the script to the init.d directory & make executable

    sudo mv init-deb.sh /etc/init.d/nginx
    sudo chmod +x /etc/init.d/nginx
    sudo /usr/sbin/update-rc.d -f nginx defaults

Restart Nginx

    sudo service nginx start

Configure vhost

    sudo vim /etc/nginx/nginx.conf
    server {
        listen       80;
        server_name  localhost demo.boocafe.vn  128.199.112.154;

        passenger_enabled on;
        passenger_min_instances 2;
        root /home/webapp/boocafe-ror/current/public;

        location ~ ^/(assets)/ {
            root /home/webapp/boocafe-ror/current/public;
            gzip_static on;
            expires max;
            add_header Cache-Control public;
        }

        access_log /home/webapp/boocafe-ror/current/public/log/access.log;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

Check run level to start on
    
    sysv-rc-conf --list postgresql

# Install NodeJS

    sudo apt-get -y install nodejs nodejs-legacy npm
    npm install -g bower

# Install Redis

    sudo apt-get install -y build-essential
    sudo apt-get install -y tcl8.5
    cd /tmp
    wget http://download.redis.io/releases/redis-stable.tar.gz
    tar xzf redis-stable.tar.gz
    cd redis-stable
    make
    make test
    sudo make install
    cd utils
    sudo ./install_server.sh

    sudo service redis_6379 start
    sudo service redis_6379 stop

# Setup Mina for deploying RoR app

Add to Gemfile

    gem 'mina'

Then, bundle install

    bundle install

Init mina config

    mina init

Edit the config/deploy.rb with your own, then run the command bellow to setup server directory structure (just run once)

    mina setup --verbose

To deploy

    mina deploy --trace

Source: https://www.digitalocean.com/community/tutorials/how-to-use-mina-to-deploy-a-ruby-on-rails-application

Set the secret key base

Generate secret key base

    cd /path-to/rails-app
    rake rescret RAILS_ENV=production

Set the ENV variable

    export SECRET_KEY_BASE="generated-secret-key-base"

Restart the site

    touch myapp/current/tmp/restart.txt

# Install PHP

Install PHP

    sudo apt-get -y install php5-fpm

Update configuration

    sudo nano /etc/php5/fpm/php.ini

Find the line, “cgi.fix_pathinfo=1” and change the “1” to “0”:

    cgi.fix_pathinfo=0

Next:

    sudo nano /etc/php5/fpm/pool.d/www.conf

Configure like this:

    listen = /var/run/php5-fpm.sock
    listen.owner = www-data
    listen.group = www-data
    listen.mode = 0777

Let’s restart PHP:

    sudo service php5-fpm restart

Configure vhosts

Open /opt/nginx/conf/nginx.conf, and update like this

    #user  nobody;
    worker_processes  1;

    #error_log  logs/error.log;
    #error_log  logs/error.log  notice;
    #error_log  logs/error.log  info;

    #pid        logs/nginx.pid;


    events {
        worker_connections  1024;
    }


    http {
        passenger_root /home/webapp/.rbenv/versions/2.2.3/lib/ruby/gems/2.2.0/gems/passenger-5.0.21;
        passenger_ruby /home/webapp/.rbenv/versions/2.2.3/bin/ruby;

        include       mime.types;
        default_type  application/octet-stream;

        #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
        #                  '$status $body_bytes_sent "$http_referer" '
        #                  '"$http_user_agent" "$http_x_forwarded_for"';

        #access_log  logs/access.log  main;

        sendfile        on;
        #tcp_nopush     on;

        #keepalive_timeout  0;
        keepalive_timeout  65;

        #gzip  on;

        include /opt/nginx/sites-enabled/*;   
    }

Then create two directory
    
    mkdir /opt/nginx/sites-available
    mkdir /opt/nginx/sites-enabled

Add vhost configuration for rails app

    vi /opt/nginx/sites-available/rails_app

    server {
        listen       80;
        server_name example1.com;

        passenger_enabled on;
        passenger_min_instances 2;
        root /home/webapp/rails_app/current/public;

        location ~ ^/(assets)/ {
            root /home/webapp/rails_app/current/public;
            gzip_static on;
            expires max;
            add_header Cache-Control public;
        }

        access_log /home/webapp/rails_app/current/public/log/access.log;

        #charset koi8-r;

        access_log  /home/webapp/rails_app/current/logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
    

Add vhost configuration for PHP app

    server {
        listen   80; ## listen for ipv4; this line is default and implied
        #listen   [::]:80 default ipv6only=on; ## listen for ipv6

        root /home/webapp/blog/;
        index index.php index.html index.htm;

        server_name example2.com;

        access_log  /var/log/example2.com.access.log;
        error_log /var/log/example2.com.error.log;

        location / {
            try_files $uri $uri/ /index.html;
        }

        error_page 404 /404.html;

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
            root /opt/nginx/html;
        }

        # pass the PHP scripts to FastCGI server listening on the php-fpm socket
        location ~ \.php$ {
            try_files $uri =404;
            fastcgi_pass unix:/var/run/php5-fpm.sock;
            fastcgi_index index.php;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include fastcgi_params;
        }
    }

Then restart nginx
    
    /etc/init.d/nginx restart

# Install monit

    sudo apt-get install monit
    sudo nano /etc/monit/monitrc

Config monit
    
    /etc/monit/conf.d/nginx▽
    check process nginx with pidfile "/opt/nginx/logs/nginx.pid"
        start program = "/etc/init.d/nginx start"
        stop program = "/etc/init.d/nginx stop"
        if failed host 127.0.0.1 port 80 then restart

    /etc/monit/conf.d/redis
    check process redis-server with pidfile "/var/run/redis_6379.pid"
        start program = "/etc/init.d/redis_6379 start"
        stop program = "/etc/init.d/redis_6379 stop"
        if failed host 127.0.0.1 port 6379 then restart

    /etc/monit/conf.d/postgres
    check process postgresql with pidfile "/var/run/postgresql/9.3-main.pid"
        start program = "/etc/init.d/postgresql start"
        stop program = "/etc/init.d/postgresql stop"
        if failed host 127.0.0.1 port 5432 then restart

    /etc/monit/conf.d/sidekiq
    check process sidekiq with pidfile "/home/webapp/boocafe-ror/tmp/pids/sidekiq.pid"
        start program = "/etc/init.d/sidekiq start"
        stop program = "/etc/init.d/sidekiq stop"

Check the syntax

    monit -t

After resolving any possible syntax errors, you can start running all of the monitored programs

    monit start all

References:
 * https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04
 * https://www.digitalocean.com/community/tutorials/how-to-install-and-use-redis
 * https://www.digitalocean.com/community/tutorials/lemp-stack-monitoring-with-monit-on-ubuntu-14-04
 * https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-monit
 * http://cdyer.co.uk/blog/init-script-for-sidekiq-with-rbenv
 * http://wiki.nginx.org/FullExample
 * https://gorails.com/deploy/ubuntu/14.04
 * https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-12-04
