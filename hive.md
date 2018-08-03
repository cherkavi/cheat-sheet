## Hive
fixed data structure ( Pig - free data structure )
[documentation](https://cwiki.apache.org/confluence/display/Hive)
[description](https://maprdocs.mapr.com/51/Hive/Hive.html)
[description](https://hortonworks.com/apache/hive/)
[cheat sheet](https://hortonworks.com/blog/hive-cheat-sheet-for-sql-users/)
[sql to hive](https://www.slideshare.net/hortonworks/sql-to-hive)
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
```
hive
```
new interpreter
```
beeline
```

### show all databases
```
show databases;
use database default;
```

### show all tables for selected database
```
show tables;
```

### create table
[documentation](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL)

---
plain text format
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
and load data into it
```
LOAD DATA INPATH '/home/user/my-prepared-data/' OVERWRITE INTO TABLE apachelog;
LOAD DATA LOCAL INPATH '/data/' OVERWRITE INTO TABLE apachelog;
```
data will be saved into: /user/hive/warehouse
if cell has wrong format - will be 'null'

---
create table from [csv](https://www.kaggle.com/passnyc/data-science-for-good)
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

do not specify filename !!!!
( all files into folder will be picked up )
```
---
insert data into another table
```
INSERT OVERWRITE TABLE <table destination>
-- CREATE TABLE <table destination>
SELECT <field1>, <field2>, ....
FROM <table source> s JOIN <table source another> s2 ON s.key_field=s2.key_field2
-- LEFT OUTER
-- FULL OUTER
```

---
CSV format
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
table from 'tab' delimiter
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
functions
```
split - split string
explode - flat map, array to separated fields
```

### UDF, custom functions
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

### troubleshooting
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

