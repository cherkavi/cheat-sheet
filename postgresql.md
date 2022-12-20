## Tools
* [cli for databases](https://github.com/dbcli)  
* [binaries to download](https://www.enterprisedb.com/download-postgresql-binaries)  

### chmod for exec files
```
chmod +x %pgsql%/bin/*
```

### replace 'text link' with files 
```
%pgsql%/lib/*.so
```

### create cluster
```
./initdb -U postgres -A password -E utf8 -W -D /dev/shm/pgsql-data/data
```
The command line parameters of the initdb command are described in following:
* -U postgres means that the superuser account of your database is called ‘postgres’.
* -A password means that password authentication is used.
* -E utf8 means that the default encoding will be UTF-8.
* -W means that you will enter the superuser password manually.
* -D /dev/shm/pgsql-data/data specifies the data directory of your PostgreSQL installation.

Issue:
```
/initdb: /lib64/libc.so.6: version `GLIBC_2.12' not found (required by /dev/shm/pgsql/bin/../lib/libldap_r-2.4.so.2)
```
solution:
```
version of your glibc is older than compiled code - decrease version of postgres
```
must work:
```
./postgres -V
```


### start DB
```
./pg_ctl -D "/dev/shm/pgsql-data/data" -l "/dev/shm/pgsql-log/pgsql.log" start
```

### stop DB
```
./pg_ctl -D "/dev/shm/pgsql-data/data" -l "/dev/shm/pgsql-log/pgsql.log" stop
```


### change access from external addresses
find /dev/shm/pgsql-data/data -name "postgresql.conf"
```
listen_addresses = '*'
```
find /dev/shm/pgsql-data/data -name "pg_hba.conf"
```
host    all             all              0.0.0.0/0                       md5
host    all             all              ::/0                            md5
```

### connect to db
#### cli installation
```sh
pip install -U pgcli
sudo apt install pgcli
```
#### postgresql cli connection
```sh
PG_HOST=localhost
PG_PORT=5432
PG_USER=postgres-prod
PG_PASS=my_secure_pass
PG_DB=data-production

psql -h $PG_HOST -p $PG_PORT -U $PG_USER $PG_DB
```
parsing of jdbc url
```sh
PG_HOST=`echo $POSTGRES_URL | awk -F '//' '{print $2}' | awk -F ':' '{print $1}'`
PG_PORT=`echo $POSTGRES_URL | awk -F '//' '{print $2}' | awk -F ':' '{print $2}' | awk -F '/' '{print $1}'`
PG_DB=`echo $POSTGRES_URL | awk -F '//' '{print $2}' | awk -F ':' '{print $2}' | awk -F '/' '{print $2}'`
echo $PG_DB"   "$PG_HOST":"$PG_PORT
export PGPASSWORD=$POSTGRES_PASSWORD
psql -h $PG_HOST  -p $PG_PORT -U $POSTGRES_USER $PG_DB
```

#### save results to file
```
# pgcli save query result
# \o [filename]                        | Send all query results to file.
```
```sql
\copy (select sku from testaccount01_variant) to 'db-2.sku' csv header;
COPY tablename TO '/tmp/output.csv' DELIMITER ',' CSV HEADER;
```

### jdbc url
```
url:
    jdbc:postgresql:database
    jdbc:postgresql://host/database
    jdbc:postgresql://host:port/database
```

### [jdbc driver](https://jdbc.postgresql.org/download.html)
```
<dependency>
    <groupId>postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>9.1-901-1.jdbc4</version>
</dependency>
```

## DB structure
![image](https://user-images.githubusercontent.com/8113355/178947493-39da3c5e-5e0b-48b2-8fbc-d704ddc5ca75.png)


## DB requests
### install client
```
sudo apt install postgresql-client-common
sudo apt-get install postgresql-client-12
```

### list of all databases, ad-hoc
```sh
psql --username postgres --list
```

### print current user
```sql
select current_user;
```

### execute query, ad-hoc check connection 
```sh
psql -w -U user_name -d database_name -c "SELECT 1"
```
### execute prepared sql file
```
psql -w -U user_name -d database_name -a -f /path/to/file.sql
psql -h ${DB_VARIANT_HOST} -p ${DB_VARIANT_PORT} -U ${DB_VARIANT_USERNAME} -f query.sql
```

### execute sql file without password 
```sh
# option 1 - via pgpass file
echo "${DB_VARIANT_HOST}:${DB_VARIANT_PORT}:${DB_VARIANT_DATABASE}:${DB_VARIANT_USERNAME}:${DB_VARIANT_PASSWORD}" > ~/.pgpass
chmod 0600 ~/.pgpass

# option 2 - via export variable - has precedence over pgpass file
unset PGPASSWORD
export PGPASSWORD=$DB_VARIANT_PASSWORD
echo $PGPASSWORD

psql -h ${DB_VARIANT_HOST} -p ${DB_VARIANT_PORT} -d ${DB_VARIANT_DATABASE} -U ${DB_VARIANT_USERNAME} -f clean-airflow.sql
```

### connect to db 
```sh
# connect
psql -U workflowmonitoring -d workflowmonitoringdb
# exit
\q
```

### command inside client
```sql
-- help
\h
\?

-- list of all databases
\l
-- list of all schemas 
\dn
-- list of all tables
---- CREATE TABLE testaccount01_variant( id INTEGER PRIMARY KEY, variant_key VARCHAR(64));
SELECT table_name FROM information_schema.tables WHERE table_schema='public';
-- list of all tables
\dt
-- list of all tables by schema ( dot at the end is must !!! )
\dt my_schema.
\dt my_schema.table1
SELECT table_name FROM information_schema.tables WHERE table_schema = 'my_schema' and table_name='table1';

-- list of all views
\dv
-- describe object
\d my_table_name

-- save output of query to file
\o
-- execute external file
\i 
-- execute command line
\!
```

### get version
```sql
SHOW server_version;
SELECT version();
```

### records in table statistics of table
```sql
SELECT * FROM pg_stat_all_tables WHERE relname = 'user_table_name';
```

### list of tables and size in bytes
```sql
select schemaname as table_schema,
       relname as table_name,
       pg_size_pretty(pg_relation_size(relid)) as table_size
from pg_catalog.pg_statio_user_tables
order by pg_relation_size(relid) desc;
```

### internal tables
```
select * from pg_tables;
SELECT schemaname, relname FROM pg_stat_user_tables;  
```

### create database create user create role
```sql
CREATE USER $DB_VARIANT_USERNAME WITH PASSWORD '$DB_VARIANT_PASSWORD'
GRANT $DB_VARIANT_USERNAME TO main_database;
CREATE DATABASE $DB_VARIANT_DATABASE OWNER $DB_VARIANT_USERNAME;
-- 
CREATE USER qa_read_only_xxxxx WITH PASSWORD 'xxxxxxx';
-- grant read only access 
GRANT connect ON database w5a823c88301e9 TO qa_read_only_xxxxx;
GRANT select on all tables in schema public to qa_read_only_xxxxx;
```

### Common operations
```sql
-- create schema
CREATE SCHEMA IF NOT EXISTS airflow_02;
DROP SCHEMA IF EXISTS airflow_02;

-- print all schemas
select s.nspname as table_schema,
       s.oid as schema_id,  
       u.usename as owner
from pg_catalog.pg_namespace s
join pg_catalog.pg_user u on u.usesysid = s.nspowner
order by table_schema;

-- list of all tables in schema ( add dot to the end !!!)
SELECT table_name FROM information_schema.tables WHERE table_schema = 'my_schema' and table_name='table1';

-- list of all columns in table
select table_schema,
       table_name,
       ordinal_position as position,
       column_name,
       data_type,
       case when character_maximum_length is not null
            then character_maximum_length
            else numeric_precision end as max_length,
       is_nullable,
       column_default as default_value
from information_schema.columns
where table_schema = 'my_schema' and table_name='table1'
order by table_schema, 
         table_name,
         ordinal_position;
```

```sql
-- row number
select ROW_NUMBER () OVER (ORDER BY sku), sku from w60be740099999931852ba405_variant group by sku;
```

```sql
-- add column to table
ALTER TABLE schema_name.table_name ADD column_name bigint;
```

### obtain DDL of table description 
```sh
pg_dump --host $PG_HOST --port $PG_PORT --username $PG_USER --dbname $PG_DB --schema-only --schema=public --table=schema_name.table_name

# pg_dump: error: aborting because of server version mismatch
# --ignore-version
# `select version();` PostgreSQL 13.2 on x86_64-pc-linux-gnu, compiled by gcc (GCC) 8.3.1 20191121 (Red Hat 8.3.1-5), 64-bit
# `pg_lsclusters`
# --cluster 13.2
```

```sql
\dt+ schema_name.table_name
\d+ schema_name.table_name
```

## Specific types
```sql
-- varying is alias for varchar
ADD COLUMN IF NOT EXISTS modifiedBy character varying(10485760) COLLATE pg_catalog."default"
-- the same as 
ADD COLUMN IF NOT EXISTS modifiedBy character varying COLLATE pg_catalog."default"
```
