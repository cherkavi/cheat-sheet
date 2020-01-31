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
