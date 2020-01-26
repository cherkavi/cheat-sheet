# DataSets
[global search for datasets](https://datasetsearch.research.google.com/)  
[datasets on kaggle](https://www.kaggle.com/datasets)  
---
# BigSQL
![overview](https://s19.postimg.cc/lmevs5koz/bigsql-overview.png)
[how to run query](https://www.ibm.com/support/knowledgecenter/en/SSPT3X_4.0.0/com.ibm.swg.im.infosphere.biginsights.analyze.doc/doc/bigsql_run_queries.html)
## Warehouse
default directory (/special-folder/hive/warehouse) in HDFS to store tables 
## Schema
directory (/special-folder/hive/warehouse/user-personal-schema) to store tables ( can be organized in schema )
## Table
directory (/special-folder/hive/warehouse/user-personal-schema/my-table-01) with zero or more files
## Partitions
additional sub-directory to save special data for the table 
```
/special-folder/hive/warehouse/user-personal-schema/my-table-01/my-column-country=USA
/special-folder/hive/warehouse/user-personal-schema/my-table-01/my-column-country=GERMANY
```
## commands
```
bigsql/bin start
bigsql/bin stop
bigsql/bin status
```
## configuration
```
bigsql-conf.xml
```

## switch on compatability mode
use Big SQL 1.0 into Big SQL 
```
set syshadoop.compatability_mode=1;
```

##  Data types
![compare data types](https://s19.postimg.cc/vg9eqx4wz/bigsql-datatypes.png)
* Declared type
* SQL type
* Hive type

### using strings
* avoid to use string - default value 32k
* change default string length
```
set hadoop property bigsql.string.size=128
```
* use VARCHAR instead of

### datetime ( not date !!! )
```
2003-12-23 00:00:00.0
```
### boolean 
![boolean workaround](https://s19.postimg.cc/6biedpfsz/bigsql-datatypes-boolean.png)

## create schema
```
create schema "my_schema";
use "my_schema"
drop schema "my_schema" cascade;
```

## create table ( @see hive.md )
```
create hadoop table IF NOT EXISTS my_table_into_my_schema ( col1 int not null primary key, col2 varchar(50)) 
row format delimited 
fields terminated by ',' 
LINES TERMINATED by '\n'
escaped BY '\\', 
null defined as '%THIS_IS_NULL%' s
stored as [<empty>, TEXT, BINARY] SEQUENCEFILE;
-- PARQUETFILE
-- ORC
-- RCFILE
-- TEXTFILE
```
avro table creation:
![avro table](https://s19.postimg.cc/bfe9fh0tf/bigsql-table-avro.png)

## insert 
* insert values (not to use for prod) - each command will create its personal file with records
```
insert into my_table_into_my_schema values (1,'first'), (2,'second'), (3,'third');
```
* file insert - copy file into appropriate folder ( with delimiter between columns )
```
call syshadoop.hcat_cache_sync('my_schema', 'my_table_into_my_schema');
```
* create table based on select
```
CREATE HADOOP TABLE new_my_table STORED AS PARQUETFILE AS SELECT * FROM my_table_into_my_schema;
```
* load data
```
LOAD HADOOP USING FILE URL 'sftp://my_host:22/sub-folder/file1' 
WITH SOURCE PROPERTIES ('field.delimiter' = '|')
INTO TABLE my_table_into_my_schema APPEND;
```

## null value
* BigSQL 1.0 - ''
* BigSQL - \N
* can be specified as part of table creation
```
NULL DEFINED AS 'null_representation'
```

## JSqsh
CLI tool to work with any JDBC driver ( Java SQl SH )
* [getting started](https://github.com/scgray/jsqsh/wiki/Getting-Started)
* [command line parameters](https://github.com/scgray/jsqsh/wiki/jsqsh)
* [ibm documentation](https://www.ibm.com/support/knowledgecenter/SSPT3X_3.0.0/com.ibm.swg.im.infosphere.biginsights.analyze.doc/doc/bsql_jsqsh.html)

