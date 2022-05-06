# SQLite 
* (tutorial)[https://alphacodingskills.com/sqlite/sqlite-tutorial.php]

## init db
```sh
sqlite3 -init db.sqlite
```

## init db and import 
```sh
# [pip3 install termsql](https://github.com/tobimensch/termsql)
termsql -i mycsvfile.CSV -d ',' -c 'field_name,field_index' -t 'index_de' -o mynewdatabase.db
```

## open db
```sh
sqlite3 mynewdatabase.db
.tables
```
## redirect output to file redirect output to stdout
```
.output result-output.txt
select * from index_de;
.output
```

## create table and import
```sql
CREATE TABLE index_de(field_name TEXT NOT NULL, field_type TEXT NOT NULL );
CREATE TABLE index_cn(field_name TEXT NOT NULL, field_type TEXT NOT NULL );
-- show tables 
.tables
.schema index_cn

-- import csv to table 
.mode csv
.separator ","
.import autolabel-staging-merge.elk-index.cn.fields.csv index_cn
.import autolabel-merge.elk-index.de-prod.fields.csv index_de
```

## sqlite export to file
```sql
.output autolabel-merge.fields
.shell ls -la autolabel-merge.fields
.shell cat autolabel-merge.fields

select index_cn.field_name, index_de.field_type "type_de", index_cn.field_type "type_cn"
from index_cn index_cn
inner join index_de on index_de.field_name=index_cn.field_name 
where index_cn.field_type != index_de.field_type
;
.output stdout
.exit
```
