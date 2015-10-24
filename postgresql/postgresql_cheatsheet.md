
Open psql shell
   
    sudo -u postgres psql postgres

Create Role

    postgres=# CREATE ROLE <role_name> WITH LOGIN PASSWORD '<password>';

Create database

    postgres=# CREATE DATABASE <database_name> OWNER <role_name>;

List all databases

    \l

List all tables
    
    \dt

View table detail

    \d+ <table_name>

List of roles

    \du

List of user mappings

    \deu+

More useful commands

    postgres=# ALTER ROLE <role_name> WITH CREATEDB;
    postgres=# ALTER ROLE <role_name> PASSWORD '<new_password>';
    postgres=# GRANT ALL PRIVILEGES ON DATABASE <database_name> TO <role_name>;
    postgres=# ALTER USER <role_name> WITH SUPERUSER;
