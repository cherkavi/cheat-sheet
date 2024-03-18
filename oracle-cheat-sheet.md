# Oracle cheat sheet
## oracle cli, sql developer command line
```sh
sudo apt -y install sqlcl-package
```
or manually via [webui](https://www.oracle.com/database/sqldeveloper/technologies/sqlcl/download/)

connection
```sh
ORACLE_USER=my_login
ORACLE_PASS='my_pass'
ORACLE_HOST=my_host
ORACLE_PORT=1953
ORACLE_SERVICE=prima2
/home/soft/sqlcl/bin/sql ${ORACLE_USER}/${ORACLE_PASS}@${ORACLE_HOST}:${ORACLE_PORT}:${ORACLE_SERVICE}
# /home/soft/sqlcl/bin/sql ${ORACLE_USER}/${ORACLE_PASS}@${ORACLE_HOST}:${ORACLE_PORT}/${ORACLE_SERVICE}
```

## [my oracle snippets](https://github.com/cherkavi/database)
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
