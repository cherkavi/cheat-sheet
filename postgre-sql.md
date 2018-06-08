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