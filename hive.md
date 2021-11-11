## Hive
fixed data structure ( Pig - free data structure )
[documentation](https://cwiki.apache.org/confluence/display/Hive)
[description](https://maprdocs.mapr.com/51/Hive/Hive.html)
[description](https://hortonworks.com/apache/hive/)
[cheat sheet](https://hortonworks.com/blog/hive-cheat-sheet-for-sql-users/)
[sql to hive](https://www.slideshare.net/hortonworks/sql-to-hive)
[quick guide](https://www.tutorialspoint.com/hive/hive_quick_guide.htm)

*not supported full SQL, especially:"
- transactions
- materialized view
- update
- non-equality joins

### metastore
HCatalog
can be one of:
* embedded 
in-process metastore
in-process database
* local
in-process metastore
out-of-process database
* remote
out-of-process metastore
out-of-process database

### hive command line interfaces
[cheat sheet](https://hortonworks.com/blog/hive-cheat-sheet-for-sql-users/)
run interpreter
```sh
hive
```
run existing script into file
```sh
hive -f <filename>
```
new interpreter
```sh
beeline
```

## Data units
Database
namespace for tables separation
-> Table
   unit of data inside some schema
  -> Partition
     virtual column ( example below )
    -> Buckets
       data of column can be divided into buckets based on hash value
Partition and Buckets serve to speed up queries during reading/joining

example of bucket existence
```
 database -> $WH/testdb.db
    table -> $WH/testdb.db/T
partition -> $WH/testdb.db/T/date=01012013
   bucket -> $WH/testdb.db/T/date=01012013/000032_0
( only 'bucket' is a file )
```


### databases
```
SHOW DATABASES;
USE DATABASE default;
-- describe
DESCRIBE DATABASE my_own_database;
DESCRIBE DATABASE EXTENDED my_own_database;
-- delete database
DROP DATABASE IF EXISTS my_own_database;
-- alter database
ALTER DATABASE my_own_database SET DBPROPERTIES(...)

```

### show all tables for selected database
```
SHOW TABLES;
```

## DDL

### types primitive
TINYINT
SMALLINT
INT
BIGINT
BOOLEAN ( TRUE/FALSE )
FLOAT
DOUBLE
DECIMAL
STRING
VARCHAR
TIMESTAMP ( YYYY-MM-DD HH:MM:SS.ffffffff )
DATE ( YYYY-MM-DD )
```
cast ( string_column_value as FLOAT )
```
### types comples
* Arrays
```
array('a1', 'a2', 'a3')
```
* Structs
```
struct('a1', 'a2', 'a3')
```
* Maps
```
map('first', 1, 'second', 2, 'third', 3)
```
* Union
```
create_union
```

### create table
[documentation](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL)
table types:
* managed
data stored in subdirectories of 'hive.metastore.warehouse.dir'
dropping managed table will drop all data on the disc too
* external
data stored outsice 'hive.metastore.warehouse.dir'
dropping table will delete metadata only
'''
CREATE EXTERNAL TABLE ...
...
LOCATION '/my/path/to/folder'
'''

---
create managed table with regular expression
```
CREATE TABLE apachelog (
  host STRING,
  identity STRING,
  user STRING,
  time STRING,
  request STRING,
  status STRING,
  size STRING,
  referer STRING,
  agent STRING)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.RegexSerDe'
WITH SERDEPROPERTIES (
  "input.regex" = "([^]*) ([^]*) ([^]*) (-|\\[^\\]*\\]) ([^ \"]*|\"[^\"]*\") (-|[0-9]*) (-|[0-9]*)(?: ([^ \"]*|\".*\") ([^ \"]*|\".*\"))?"
)
STORED AS TEXTFILE;
```
create managed table with complex data
```
CREATE TABLE users{
id INT,
name STRING,
departments ARRAY<STRING>
} ROW FORMAT DELIMITED FIELD TERMINATED BY ','
            COLLECTION ITEMS TERMINATED BY ':'
STORED AS TEXTFILE;

1, Mike, sales|manager
2, Bob,  HR
3, Fred, manager| HR
4,Klava, manager|sales|developer|cleaner

```

create managed table with partition
```
CREATE TABLE users{
id INT,
name STRING,
departments ARRAY<STRING>
}
 PARTITIONED BY (office_location STRING ) 
 ROW FORMAT DELIMITED FIELD TERMINATED BY ','
            COLLECTION ITEMS TERMINATED BY ':'
STORED AS TEXTFILE;
--
-- representation on HDFS
$WH/mydatabase.db/users/office_location=USA
$WH/mydatabase.db/users/office_location=GERMANY
```
---
create external table from [csv](https://www.kaggle.com/passnyc/data-science-for-good)
CSV format
```
CREATE EXTERNAL TABLE IF NOT EXISTS school_explorer(
	grade boolean,
	is_new boolean, 
	location string,
	name string, 
	sed_code STRING,
	location_code STRING, 
	district int,
	latitude float,
	longitude float,
	address string
)COMMENT 'School explorer from Kaggle'
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
STORED AS TEXTFILE LOCATION '/data/';
-- do not specify filename !!!!
-- ( all files into folder will be picked up )
```
---
create table from CSV format file
```
CREATE TABLE my_table(a string, b string, ...)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.OpenCSVSerde'
WITH SERDEPROPERTIES (
   "separatorChar" = "\t",
   "quoteChar"     = "'",
   "escapeChar"    = "\\"
)  
STORED AS TEXTFILE LOCATION '/data/';
```
---
create table from 'tab' delimiter
```
CREATE TABLE web_log(viewTime INT, userid BIGINT, url STRING, referrer STRING, ip STRING) 
ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'; 
LOAD DATA LOCAL INPATH '/home/mapr/sample-table.txt' INTO TABLE web_log;
```
---
JSON
```
CREATE TABLE my_table(a string, b bigint, ...)
ROW FORMAT SERDE 'org.apache.hive.hcatalog.data.JsonSerDe'
STORED AS TEXTFILE;
```
---
external Parquet
```
create external table parquet_table_name (x INT, y STRING)
  ROW FORMAT SERDE 'parquet.hive.serde.ParquetHiveSerDe'
  STORED AS 
    INPUTFORMAT "parquet.hive.DeprecatedParquetInputFormat"
    OUTPUTFORMAT "parquet.hive.DeprecatedParquetOutputFormat"
    LOCATION '/test-warehouse/tinytable';
```
---
drop table
```
DROP TABLE IF EXISTS users;
```
---
alter table - rename
```
ALTER TABLE users RENAME TO external_users;
```
alter table - add columns
```
ALTER TABLE users ADD COLUMNS{
age INT, children BOOLEAN
}
```
---
View
```
CREATE VIEW young_users SELECT name, age FROM users WHERE age<21;
DROP VIEW IF EXISTS young_users;
```


# Index
create index for some specific field, will be saved into separate file
```
CREATE INDEX users_name ON TABLE users ( name ) AS 'users_name';
```
show index
```
SHOW INDEX ON users;
```
delete index
```
DROP INDEX users_name on users;
```

# DML 
---
load data into table
```
-- hdfs
LOAD DATA LOCAL INPATH '/home/user/my-prepared-data/' OVERWRITE INTO TABLE apachelog;
-- local file system
LOAD DATA INPATH '/data/' OVERWRITE INTO TABLE apachelog;
-- load data with partitions, override files on hdfs if they are exists ( without OVERWRITE )
LOAD DATA INPATH '/data/users/country_usa' INTO TABLE users PARTITION (office_location='USA', children='TRUE')
-- example of partition location: /user/hive/warehouse/my_database/users/office_location=USA/children=TRUE
```
data will be copied and saved into: /user/hive/warehouse
if cell has wrong format - will be 'null'
---
insert data into table using select, insert select
```
INSERT OVERWRITE TABLE <table destination>
-- INSERT OVERWRITE TABLE <table destination>
-- CREATE TABLE <table destination>
SELECT <field1>, <field2>, ....
FROM <table source> s JOIN <table source another> s2 ON s.key_field=s2.key_field2
-- LEFT OUTER
-- FULL OUTER
```
---
export data from Hive, data external copy, data copy
```
INSERT OVERWRITE LOCAL DIRECTORY '/home/users/technik/users-db-usa'
SELECT name, office_location, age
FROM users
WHERE office_location='USA'
```
select 
```
SELECT * FROM users LIMIT 1000;
SELECT name, department[0], age FROM users;
SELECT name, struct_filed_example.post_code FROM users ORDER BY age DESC;
SELECT .... FROM users GROUP BY age HEAVING MIN(age)>50
-- from sub-query
FROM ( SELECT * FROM users WHERE age>30 ) custom_sub_query SELECT custom_sub_query.name, custom_sub_query.office_location WHERE children==FALSE;

```

---
# functions
```
-- if regular expression B can be applied to A
A RLIKE B
A REGEXP B
-- split string to elements
split
-- flat map, array to separated fields - instead of one field with array will be many record with one field
explode( array field )
-- extract part of the date: year, month, day
year(timestamp field)
-- extract json object from json string
get_json_object
-- common functions with SQL-92
A LIKE B
round
ceil
substr
upper
Length
count
sum
average
```

# user defined functions
## types
* UDF
* UDAggregatedFunctions
* UDTablegeneratingFunctions

## UDF, custom functions
```
<dependencies>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-client</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.hive</groupId>
        <artifactId>hive-exec</artifactId>
        <version>1.2.1</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```
```
package com.mycompany.hive.lower;
import org.apache.hadoop.hive.ql.exec.Description;
import org.apache.hadoop.hive.ql.exec.UDF;
import org.apache.hadoop.io.*;

// Description of the UDF
@Description(
    name="ExampleUDF",
    value="analogue of lower in Oracle .",
    extended="select ExampleUDF(deviceplatform) from table;"
)
public class ExampleUDF extends UDF {
    public String evaluate(String input) {
        if(input == null)
            return null;
        return input.toLowerCase();
    }
}
```
after compillation into my-udf.jar
```
  hive> addjar my-udf.jar
  hive> create temporary function ExampleUDF using "com.mycompany.hive.lower";
  hive> SELECT ExampleUDF(value) from table;
```

#Streaming
MAP(), REDUCE(), TRANSFORM()
```
SELECT TRANSFORM (name, age) 
USING '/bin/cat'
AS name, age FROM my_own_database.users;
```

# troubleshooting
query explanation and understanding of the DirectAsyncGraph
```
EXPLAIN SELECT * FROM users ORDER BY age DESC;
EXPLAIN EXTENDED SELECT * FROM users ORDER BY age DESC;
```
---
jdbc connection issue:
```
TApplicationException: Required field 'client_protocol' is unset! 
```
reason:
```
This indicates a version mismatch between client and server, namely that the client is newer than the server, which is your case.
```
solution:
```
need to decrease version of the client
    compile group: 'org.apache.hive', name: 'hive-jdbc', version: '1.1.0'
```


### hive html gui
- ambari
- hue

