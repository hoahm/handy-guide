
## Open psql shell
   
    sudo -u postgres psql postgres

## Create Role

    postgres=# CREATE ROLE <role_name> WITH LOGIN PASSWORD '<password>';

## Create database

    postgres=# CREATE DATABASE <database_name> OWNER <role_name>;

## List all databases

    \l

## List all tables

    \dt

## View table detail

    \d+ <table_name>

## List of roles

    \du

## List of user mappings

    \deu+

## More useful commands

    postgres=# ALTER ROLE <role_name> WITH CREATEDB;
    postgres=# ALTER ROLE <role_name> PASSWORD '<new_password>';
    postgres=# GRANT ALL PRIVILEGES ON DATABASE <database_name> TO <role_name>;
    postgres=# ALTER USER <role_name> WITH SUPERUSER;

## List all databases

    \l

## Show all databases' size

    SELECT pg_database.datname, pg_database_size(pg_database.datname), pg_size_pretty(pg_database_size(pg_database.datname)) FROM pg_database ORDER BY pg_database_size DESC;

    e=$(psql -lAt | head -n1 | cut -d\| -f1); psql -c "SELECT pg_database.datname, pg_database_size(pg_database.datname), pg_size_pretty(pg_database_size(pg_database.datname)) FROM pg_database ORDER BY pg_database_size DESC;" -d ${e}

## Show database size

    SELECT pg_size_pretty(pg_database_size('cems_dev')) As fulldbsize;

## List all schemas

    select schema_name from information_schema.schemata;

    select nspname from pg_catalog.pg_namespace;

    \dn

## Show all schemas with db sizes

    SELECT schema_name, pg_size_pretty(sum(table_size)::bigint), (sum(table_size) / pg_database_size(current_database())) * 100 AS percentage FROM (SELECT pg_catalog.pg_namespace.nspname as schema_name, pg_relation_size(pg_catalog.pg_class.oid) as table_size FROM pg_catalog.pg_class JOIN pg_catalog.pg_namespace ON relnamespace = pg_catalog.pg_namespace.oid) t GROUP BY schema_name ORDER BY schema_name;

## Show schema size

    # Create function 
    CREATE OR REPLACE FUNCTION pg_schema_size(text) RETURNS BIGINT AS $$
      SELECT SUM(pg_relation_size(quote_ident(schemaname) || '.' || quote_ident(tablename)))::BIGINT FROM pg_tables WHERE schemaname = $1
    $$ LANGUAGE SQL;

    # Call function
    select pg_size_pretty(pg_schema_size('public'));

## Show table size without indexes

    select pg_size_pretty(pg_relation_size('users'));

## Show table size with indexes

    select pg_size_pretty(pg_total_relation_size('users'));

## Show index size

    select pg_size_pretty(pg_relation_size('activities_pkey'));


## List all indexes with detail

    SELECT i.relname as indname, i.relowner as indowner, idx.indrelid::regclass, am.amname as indam, idx.indkey, ARRAY(SELECT pg_get_indexdef(idx.indexrelid, k + 1, true) FROM generate_subscripts(idx.indkey, 1) as k ORDER BY k) as indkey_names, idx.indexprs IS NOT NULL as indexprs, idx.indpred IS NOT NULL as indpred FROM pg_index as idx JOIN pg_class as i ON i.oid = idx.indexrelid JOIN pg_am as am ON i.relam = am.oid;

## List all indexes with detail (but trim the namespace)

    SELECT i.relname as indname, i.relowner as indowner, idx.indrelid::regclass, am.amname as indam, idx.indkey, ARRAY(SELECT pg_get_indexdef(idx.indexrelid, k + 1, true) FROM generate_subscripts(idx.indkey, 1) as k ORDER BY k) as indkey_names, idx.indexprs IS NOT NULL as indexprs, idx.indpred IS NOT NULL as indpred FROM pg_index as idx JOIN pg_class as i ON i.oid = idx.indexrelid JOIN   pg_am as am ON i.relam = am.oid JOIN pg_namespace as ns ON ns.oid = i.relnamespace AND ns.nspname = ANY(current_schemas(false));

## List all indexes (more friendly version)

    SELECT U.usename AS user_name, ns.nspname AS schema_name, idx.indrelid :: REGCLASS AS table_name, i.relname AS index_name, idx.indisunique AS is_unique, idx.indisprimary AS is_primary, am.amname  AS index_type, idx.indkey, ARRAY(SELECT pg_get_indexdef(idx.indexrelid, k + 1, TRUE) FROM generate_subscripts(idx.indkey, 1) AS k ORDER BY k ) AS index_keys, (idx.indexprs IS NOT NULL) OR (idx.indkey::int[] @> array[0]) AS is_functional, idx.indpred IS NOT NULL AS is_partial FROM pg_index AS idx JOIN pg_class AS i ON i.oid = idx.indexrelid JOIN pg_am AS am ON i.relam = am.oid JOIN pg_namespace AS NS ON i.relnamespace = NS.OID JOIN pg_user AS U ON i.relowner = U.usesysid WHERE NOT nspname LIKE 'pg%';

## Show list of biggest relations 

    SELECT relname AS "relation", pg_size_pretty(pg_relation_size(C.oid)) AS "size" FROM pg_class C LEFT JOIN pg_namespace N ON (N.oid = C.relnamespace) WHERE nspname NOT IN ('pg_catalog', 'information_schema') ORDER BY pg_relation_size(C.oid) DESC LIMIT 10;

## Find the total size of the biggest table

    SELECT nspname || '.' || relname AS "relation", pg_size_pretty(pg_total_relation_size(C.oid)) AS "total_size" FROM pg_class C LEFT JOIN pg_namespace N ON (N.oid = C.relnamespace) WHERE nspname NOT IN ('pg_catalog', 'information_schema') AND C.relkind <> 'i' AND nspname !~ '^pg_toast' ORDER BY pg_total_relation_size(C.oid) DESC LIMIT 20;


## Find the largest databases

    SELECT d.datname AS Name,  pg_catalog.pg_get_userbyid(d.datdba) AS Owner, CASE WHEN pg_catalog.has_database_privilege(d.datname, 'CONNECT') THEN pg_catalog.pg_size_pretty(pg_catalog.pg_database_size(d.datname)) ELSE 'No Access' END AS SIZE FROM pg_catalog.pg_database d ORDER BY CASE WHEN pg_catalog.has_database_privilege(d.datname, 'CONNECT') THEN pg_catalog.pg_database_size(d.datname) ELSE NULL END DESC LIMIT 20;


# References

* [Determining size of database, schema, tables and geometry](http://www.postgresonline.com/journal/archives/110-Determining-size-of-database,-schema,-tables,-and-geometry.html)

* [Postgresql database table indexes size](https://www.niwi.nz/2013/02/17/postgresql-database-table-indexes-size/)

* [Disk usage](https://wiki.postgresql.org/wiki/Disk_Usage)
