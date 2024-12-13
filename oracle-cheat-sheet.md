
# Oracle cheat sheet
[jdbc driver to download](https://www.oracle.com/database/technologies/appdev/jdbc-downloads.html)

## ddl er diagrams schema visualizer
* [drawdb](https://drawdb.vercel.app)
* [chartdb](https://chartdb.io)
* [Diagrams, ex Draw.io](https://Diagrams.net)
* [dbdiagram](https://dbdiagram.io)

## oracle cli, sql developer command line
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
```
set termout off
set verify off
set trimspool on
set linesize 200
set longchunksize 200000
set long 200000
set pages 0
column txt format a120
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
