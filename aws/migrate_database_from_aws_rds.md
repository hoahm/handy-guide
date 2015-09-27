Migrate database from AWS RDS
___

This guide show you how to migrate database from AWS RDS to localhost or another server.

 * [Dump database](#dump-database)
 * [Restore database](#restore-database)
 * [Transfer ownership](#transfer-ownership)
 * [Update security group](#update-security-group)  


# Dump database

    pg_dump --no-acl --no-owner -i -F c -b -v -h <db_endpoint> -U <db_user> -d <db_name>  -f ./<db_file_name>.sql


If you got error like this, see [Transfer ownership of the postgis extension](#transfer-ownership)

    pg_dump: [archiver (db)] query was: LOCK TABLE topology.topology IN ACCESS SHARE MODE

# Restore database

    psql -h <new_db_endpoint> -U <db_user> -d <db_name> < ./<db_file_name>.sql


# Transfer ownership

Follow these steps bellow or make a refer to [Setting up PostGIS](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.PostgreSQL.CommonDBATasks.html#Appendix.PostgreSQL.CommonDBATasks.PostGIS)

Step 1: Connect to DB instance 

    psql -h <db_endpoint> -U <db_user> -d <db_name>


If you can't not connect to the database instance, see [Security Group](#update-security-group)

Step 2: Check if using POSTGIS extension

    SELECT postgis_full_version();

Step 3: List all schemas

    \dn

Step 4: Transfer ownership of the extensions to the rds_superuser role


    ALTER SCHEMA topology OWNER TO rds_superuser;    

Step 5: Transfer ownership of the objects to the rds_superuser role

    CREATE FUNCTION exec(text) returns text language plpgsql volatile AS $f$ BEGIN EXECUTE $1; RETURN $1; END; $f$;
    SELECT exec('ALTER TABLE ' || quote_ident(s.nspname) || '.' || quote_ident(s.relname) || ' OWNER TO rds_superuser')
      FROM (
        SELECT nspname, relname
        FROM pg_class c JOIN pg_namespace n ON (c.relnamespace = n.oid) 
        WHERE nspname in ('topology') AND
        relkind IN ('r','S','v') ORDER BY relkind = 'S')
    s;    

# Update security group

Step 1: Go to AWS RDS Dashboard, choose the database you want to update the permission

Step 2: Click on the Security Group, a new tab open

Step 3: In the Inbound tab, add new rule:

    Type, Protocol, Port range, Source
    All traffic, All, 0-65535, Custom IP, <your-ip>
