
 * [Add swap](#swap)
 * [Config NTP](#config-ntp)
 * [User and permission](#user-and-permission)
 * [Install Ruby and all dependencies](#install-ruby-and-all-dependencies)
 * [Install PostgreSQL](#install-postgresql)
 * [Install Nginx with Passenger](#install-nginx-with-passenger)
 * [Install NodeJS](#install-nodejs)
 * [Install Redis](#install-redis)

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

# Install Ruby and all dependencies

Install dependencies

    sudo apt-get update
    sudo apt-get install -y git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev libgdbm-dev libncurses5-dev automake libtool bison libffi-dev

Install imagemagick for paperclip

    apt-get install -y imagemagick

Install RVM

    curl -L https://get.rvm.io | bash
    source ~/.rvm/scripts/rvm
    rvm install 2.2.1
    rvm use 2.2.1 --default
    ruby -v

Do not install the gems' document

    echo "gem: --no-ri --no-rdoc" > ~/.gemrc

Install Bundle and Rails

    gem install bundler
    gem install rails

# Install PostgreSQL

    sudo apt-get install postgresql postgresql-contrib libpq-dev

Create PostgreSQL's user

    sudo su - postgres
    createuser --pwprompt
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

# Install Nginx with Passenger

    gem install passenger -v 4.0.55
    rvmsudo passenger-install-nginx-module

Add Nginx startup script by downloading nginx startup script

    wget -O init-deb.sh https://www.linode.com/docs/assets/660-init-deb.sh

Move the script to the init.d directory & make executable
    
    sudo mv init-deb.sh /etc/init.d/nginx
    sudo chmod +x /etc/init.d/nginx

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

    sudo apt-get -Y install nodejs nodejs-legacy npm
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

Add nginx to the system startup
    
    sudo /usr/sbin/update-rc.d -f nginx defaults

The other ways:

    sudo apt-get install sysv-rc-conf
    sudo sysv-rc-conf postgresql on
    sudo sysv-rc-conf nginx on
    sudo sysv-rc-conf redis on
    sudo sysv-rc-conf sidekiq on

If getting the locale errors, set the env variables:

    perl: warning: Setting locale failed.
    perl: warning: Please check that your locale settings:

    LANG="en_US.UTF-8"
    LANGUAGE="en_US:en"
    LC_ALL=en_US.UTF-8

References:
 * https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04
 * https://www.digitalocean.com/community/tutorials/how-to-install-and-use-redis
 * https://www.digitalocean.com/community/tutorials/lemp-stack-monitoring-with-monit-on-ubuntu-14-04
 * http://cdyer.co.uk/blog/init-script-for-sidekiq-with-rbenv
 * http://wiki.nginx.org/FullExample
