[cli for databases](https://github.com/dbcli)
[binaries to download](https://www.enterprisedb.com/download-postgresql-binaries)

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
```sh
pip install -U pgcli
sudo apt install pgcli
pgcli --host 10.14.22.20 --port 30063 --username admin --password adminpass --dbname my_db
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

### execute request, ad-hoc
```sh
psql -w -U user_name -d database_name -c "SELECT 1"
```

### execute prepared sql file
```
psql -w -U user_name -d database_name -a -f /path/to/file.sql
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

-- 
-- list of all databases
\l
-- list of all tables
-- CREATE TABLE testaccount01_variant( id INTEGER PRIMARY KEY, variant_key VARCHAR(64));
SELECT table_name FROM information_schema.tables WHERE table_schema='public';
-- list of all tables
\dt
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
