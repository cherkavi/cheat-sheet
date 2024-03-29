# Oracle cheat sheet
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

### sqlline
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

### order by records desc, last record from table
select * from table(select * from table ORDER BY ROWNUM DESC) where rownum=1

### search into all tab columns
select * from all_tab_columns
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
