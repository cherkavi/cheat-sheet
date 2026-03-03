
# Oracle cheat sheet
## links
* [jdbc driver to download](https://www.oracle.com/database/technologies/appdev/jdbc-downloads.html)
* [linux client local installation](https://github.com/cherkavi/docker-images/blob/master/oracle/Dockerfile)
* [oracle docker]()

## ddl er diagrams schema visualizer
* [drawdb](https://drawdb.vercel.app)
* [chartdb](https://chartdb.io)
* [Diagrams, ex Draw.io](https://Diagrams.net)
* [dbdiagram](https://dbdiagram.io)

## oracle cli, sql developer command line console cli
### linux client
```sh
sudo apt-get update; sudo apt-get install libaio1 wget unzip
wget https://download.oracle.com/otn_software/linux/instantclient/2340000/instantclient-basiclite-linux.x64-23.4.0.24.05.zip
unzip instantclient-basiclite-linux.x64-23.4.0.24.05.zip
# sudo mv instantclient_23_4 /opt/oracle  # libclntsh.so
# echo "export LD_LIBRARY_PATH=$(pwd)/instantclient_23_4:$LD_LIBRARY_PATH" >> $GITHUB_ENV 
```
### sqlcl
```sh
sudo apt -y install sqlcl-package
```
or manually via [webui](https://www.oracle.com/database/sqldeveloper/technologies/sqlcl/download/)

```sh
ORACLE_USER=my_login
ORACLE_PASS='my_pass'
ORACLE_HOST=my_host
ORACLE_PORT=1953
ORACLE_SID=prima2
/home/soft/sqlcl/bin/sql ${ORACLE_USER}/${ORACLE_PASS}@${ORACLE_HOST}:${ORACLE_PORT}:${ORACLE_SID}
# /home/soft/sqlcl/bin/sql ${ORACLE_USER}/${ORACLE_PASS}@${ORACLE_HOST}:${ORACLE_PORT}/${ORACLE_SERVICE}
```
[update settings](http://ss64.com/ora/syntax-sqlplus-set.html)
```sh
# config file 
cat ${HOME}/.sqlcl/config
```
```sql
-- or inside sqlcl
SHOW SQLPATH;
```
```
set long 50000;
SET LIN[ESIZE] 200
```
```sql
set termout off
set verify off
set trimspool on
set linesize 200
set longchunksize 200000
set long 200000

set pages 0
set pagesize 0            -- No page breaks, useful for scripting
column txt format a120

set sqlformat csv         -- Output in CSV format
set heading off           -- Do not print column headers
set heading on            -- Print column headers
set feedback off          -- Suppress "X rows selected" message
set feedback on           -- Show "X rows selected" message

set linesize 1000         -- Set max line width for output
set trimspool on          -- Remove trailing spaces in spooled output
set colsep ','            -- Set column separator (for plain text output)
set termout off           -- Do not display output on screen (only spool)
set termout on            -- Display output on screen
set echo off              -- Do not echo commands
set echo on               -- Echo commands
set timing on             -- Show timing for each command
set timing off            -- Hide timing
```

#### capture/catch/write output result of sql query to file
```sql
set heading off
spool out.txt
spool off
```
```sql
set heading off
set sqlformat csv
spool out.csv
spool off
```
#### capture/catch/write output result of sql query to file via terminal 
```sh
script sql-command.output

sql .... 

exit
cat sql-command.output
```


### [sqlline](https://github.com/julianhyde/sqlline?tab=readme-ov-file#building)
```sh
# JDBC_DRIVER='oracle.jdbc.driver.OracleDrive'
JDBC_URL="jdbc:oracle:thin:@${JDBC_HOST}:${JDBC_PORT}:${JDBC_SERVICE}"
java -cp "/home/soft/sqlline/*" sqlline.SqlLine -u "${JDBC_URL}" -n "${JDBC_USER}" -p "${JDBC_PASS}"
```
### sqlplus
```sh
# https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html
# download basic
# download sql*plus
sudo apt-get install alien
sudo alien -i oracle-instantclient*-basic*.rpm
sudo alien -i oracle-instantclient*-sqlplus*.rpm

# ll /usr/lib/oracle
# ll /usr/lib/oracle/21/client64

export CLIENT_HOME=/usr/lib/oracle/21/client64
export LD_LIBRARY_PATH=$CLIENT_HOME/lib
export PATH=$PATH:$CLIENT_HOME/bin

sqlplus
```

### python connection
```py
# sqlplus user/pass@host:port:sid
dsn = oracledb.makedsn(ORACLE_HOST, int(ORACLE_PORT), sid=ORACLE_SID) #  print(dsn)
conn = oracledb.connect(user=ORACLE_USER, password=ORACLE_PASS, dsn=dsn)

############## SERVICE NAME
# sqlplus user/pass@host:port/service_name
dsn = oracledb.makedsn(ORACLE_HOST, int(ORACLE_PORT), service_name=ORACLE_SERVICE)
conn = oracledb.connect(user=ORACLE_USER, password=ORACLE_PASS, dsn=dsn)
# ----
dsn = f"{ORACLE_HOST}:{ORACLE_PORT}/{ORACLE_SERVICE}"
conn = oracledb.connect(user=ORACLE_USER, password=ORACLE_PASS, dsn=dsn)
# ----
dsn = f"{ORACLE_USER}/{ORACLE_PASS}@{ORACLE_HOST}:{ORACLE_PORT}/{ORACLE_SERVICE}"
connection = oracledb.connect(dsn)
# ----
conn = oracledb.connect(user=ORACLE_USER, password=ORACLE_PASS, service_name=ORACLE_SERVICE)

############## SID
# sql ${ORACLE_USER}/${ORACLE_PASS}@${ORACLE_HOST}:${ORACLE_PORT}:${ORACLE_SID} 
dsn = f"(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST={ORACLE_HOST})(PORT={ORACLE_PORT}))(CONNECT_DATA=(SID={ORACLE_SID})))"
conn = oracledb.connect(user=ORACLE_USER, password=ORACLE_PASS, dsn=dsn)
# ----
dsn = f"{ORACLE_HOST}:{ORACLE_PORT}:{ORACLE_SID}"
conn = oracledb.connect(user=ORACLE_USER, password=ORACLE_PASS, dsn=dsn)
# ----
dsn = f"{ORACLE_USER}/{ORACLE_PASS}@{ORACLE_HOST}:{ORACLE_PORT}:{ORACLE_SID}"
connection = oracledb.connect(dsn)
# ----
conn = oracledb.connect(user=ORACLE_USER, password=ORACLE_PASS, sid=ORACLE_SID)
```

### how to export tables ( fast way )
#### installation 
```sh
mkdir -p /home/soft/oracle
cd /home/soft/oracle
# https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html
wget https://download.oracle.com/otn_software/linux/instantclient/1930000/instantclient-basic-linux.x64-19.30.0.0.0dbru.zip
wget https://download.oracle.com/otn_software/linux/instantclient/1930000/instantclient-tools-linux.x64-19.30.0.0.0dbru.zip
unzip instantclient-basic-linux.x64-19.30.0.0.0dbru.zip
unzip instantclient-tools-linux.x64-19.30.0.0.0dbru.zip

export ORACLE_HOME=/home/soft/oracle/instantclient_19_30
export LD_LIBRARY_PATH=$ORACLE_HOME:$LD_LIBRARY_PATH
export PATH=$ORACLE_HOME:$PATH
echo $LD_LIBRARY_PATH


#### install libaio1 package 
sudo apt-get update
sudo apt-get install libaio1
# sudo apt-get install libaio       # alternative way, doesn't work
# sudo apt-get install libaio-dev   # alternative way, doesn't work    

# For Debian/Ubuntu
wget http://ftp.us.debian.org/debian/pool/main/liba/libaio/libaio1_0.3.113-4_amd64.deb
sudo dpkg -i libaio1_0.3.113-4_amd64.deb
# Or create a local symlink in Oracle directory
ldconfig -p | grep libaio
ln -s /lib/x86_64-linux-gnu/libaio.so.1t64 $ORACLE_HOME/libaio.so.1

# check if it works
ldd $ORACLE_HOME/expdp | grep libaio
```

#### execution
```sh
pushd /home/projects/temp/archiving/db-export

ORACLE_USER=$ORACLE_INT_USER
ORACLE_PASS=$ORACLE_INT_PASS
ORACLE_SERVICE=$ORACLE_INT_SERVICE
ORACLE_HOST=$ORACLE_INT_HOST
ORACLE_PORT=$ORACLE_INT_PORT

# Check which Oracle directories you have access to (without admin rights)
sql ${ORACLE_USER}/${ORACLE_PASS}@${ORACLE_HOST}:${ORACLE_PORT}:${ORACLE_SERVICE} << SQL_EOF
  SELECT directory_name, directory_path FROM all_directories;
  EXIT;
SQL_EOF

# Files will be created in the Oracle server directory (not your local machine)
# To find where files were written, check the directory path from the query above    
# if no directories available - use "slow method" 

# Create a temporary TNS entry for expdp (ORACLE_SERVICE is a SID, not service name)
mkdir -p $ORACLE_HOME/network/admin
cat > $ORACLE_HOME/network/admin/tnsnames.ora << TNS_EOF
EXPORT_DB = 
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = ${ORACLE_HOST})(PORT = ${ORACLE_PORT}))
    (CONNECT_DATA = (SID = ${ORACLE_SERVICE}))
  )
TNS_EOF

# Use DATA_PUMP_DIR (default directory, usually accessible) or replace with directory from query above
# Note: dumpfile and logfile should be filenames only (not full paths) - Oracle writes to the directory path
expdp ${ORACLE_USER}/${ORACLE_PASS}@EXPORT_DB \
  tables=A01 \
  directory=DATA_PUMP_DIR \
  dumpfile=A01.dmp \
  logfile=A01.log
```



### how to export tables ( slow way )
```sql
SET SQLFORMAT INSERT
SET FEEDBACK OFF
SET PAGESIZE 0
SET HEADING OFF
SET LINESIZE 32767

-- done
SPOOL /home/myuser/A01.sql
-- SELECT count(*) FROM A01; -- 34824205
SELECT * FROM A01;
SPOOL OFF
```

## [my oracle snippets](https://github.com/cherkavi/database)

### DDL
```sql
select DBMS_LOB.substr(dbms_metadata.get_ddl('TABLE', 'my_table'), 3000, 1) from dual;
select dbms_metadata.get_ddl('TABLE', 'my_table') from dual;
```
### using date
case( column_1 as Timestamp )

### sql plus length of the lines
set len 200

### sql plus hot keys
/
l

### sql plus, execute file and exit
echo exit | sqlplus user/pass@connect @scriptfilename

### length of blob, len of clob
dbms_lob.getlength()

### order by records desc, last record from table, limit amount of records to show
select * from ( SELECT * FROM TABLE order by rownum desc) where rownum=1;

### search into all tab columns
select * from all_tab_columns;
select * from all_triggers where trigger_name like upper('enum_to_fee_service');

### dbms output enable
BEGIN
  dbms_output.enable;
  dbms_output.put_line('hello');
END;

### 'select' combining, resultset combining
```
union
union all
intersect
minus
```

### create backup of the table
```sql
EXECUTE IMMEDIATE 'CREATE TABLE my_table AS SELECT * FROM my_original_table';
/
```

### copy all records from the table 
```sql
EXECUTE IMMEDIATE 'INSERT INTO my_table AS SELECT * FROM my_original_table';
/
```
