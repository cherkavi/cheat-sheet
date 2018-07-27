# Hadoop 
[tutorial](http://hadooptutorial.info)
[MapR documentation](https://maprdocs.mapr.com/51/WR-ecosystem-intro.html)
[Cloudera documentation](https://www.cloudera.com/documentation/enterprise/latest.html)
[Hortonworks tutorials](https://hortonworks.com/tutorials/)
[Hortonworks ecosystems](https://hortonworks.com/ecosystems/)

## Hadoop into Docker container 
* MapR
* Hortonworks
* Cloudera

### [couldera container start](https://www.cloudera.com/documentation/enterprise/latest/topics/quickstart_docker_container.html#cloudera_docker_container)
```
docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 7180 4239cd2958c6 /usr/bin/docker-quickstart
```

### couldera run:
```docker run -v /tmp:/home/root/tmp --net docker.local.network --ip 172.18.0.100 --hostname hadoop-local --network-alias hadoop-docker -t -i sequenceiq/hadoop-docker 
```

## HDFS common commands

### help ( Distributed File System )
```
hdfs dfs -help
hdfs dfs -help copyFromLocal
hdfs dfs -help ls
hdfs dfs -help cat
hdfs dfs -help setrep
```


### list files
```
hdfs dfs -ls /user/root/input
hdfs dfs -ls hdfs://hadoop-local:9000/data
```
output example:
```
-rw-r--r--   1 root supergroup       5107 2017-10-27 12:57 hdfs://hadoop-local:9000/data/Iris.csv
             ^ factor of replication
```

### change factor of replication 
```
hdfs dfs -setrep -w 4 /data/file.txt
```

### create folder
```
hdfs dfs -mkdir /data 
```

### copy files from local filesystem to remote
```
hdfs dfs -put /home/root/tmp/Iris.csv /data/
hdfs dfs -copyFromLocal /home/root/tmp/Iris.csv /data/
```

### copy files from local filesystem to remote with replication factor
```
hdfs dfs -Ddfs.replication=2 -put /path/to/local/file /path/to/hdfs
```

### copy ( small files only !!! ) from local to remote ( read from DataNodes and write to DataNodes !!!)
```
hdfs dfs -cp /home/root/tmp/Iris.csv /data/
```

### remote copy ( not used client as pipe )
```
hdfs distcp /home/root/tmp/Iris.csv /data/
```

### read data from DataNode
```
hdfs get /path/to/hdfs /path/to/local/file
hdfs dfs -copyToLocal /path/to/hdfs /path/to/local/file
```

### remove data from HDFS ( to Trash !!! special for each user)
```
hdfs rm -r /path/to/hdfs-folder
```

### remove data from HDFS
```
hdfs rm -r -skipTrash /path/to/hdfs-folder
```

### clean up trash bin
```
hdfs dfs -expunge
```

### file info ( disk usage )
```
hdfs dfs -du -h /path/to/hdfs-folder
```

### is file/folder exists ? 
```
hdfs dfs -test /path/to/hdfs-folder
```

### list of files ( / - root )
```
hdfs dfs -ls /
hdfs dfs -ls hdfs://192.168.1.10:8020/path/to/folder
```
the same as previous but with fs.defalut.name = hdfs://192.168.1.10:8020
```
hdfs dfs -ls /path/to/folder
hdfs dfs -ls file:///local/path   ==   (ls /local/path)
```
show all sub-folders
```
hdfs dfs -ls -r 
```

### standard command for hdsf
```
-touchz, -cat (-text), -tail, -mkdir, -chmod, -chown, -count ....
```

## Hadoop governance, administration
### filesystem capacity, disk usage in human readable format
```
hdfs dfs -df -h
```

### file system check, reporting, file system information
```
hdfs fsck /
```

### balancer for distributed file system, necessary after failing/removing/eliminating some DataNode(s)
```
hdfs balancer
```

### administration of the filesystem
```
hdfs dfsadmin -help
```
show statistic
```
hdfs dfsadmin -report
```
HDFS to "read-only" mode for external users
```
hdfs dfsadmin -safemode
hdfs dfsadmin -upgrade
hdfs dfsadmin -backup
```

### job execution
```
hadoop jar {path to jar} {classname}
jarn jar {path to jar} {classname}
```

---
# Hortonworks sandbox
[tutorials](https://hortonworks.com/tutorials/)
[ecosystem](https://hortonworks.com/ecosystems/)
[sandbox tutorial](https://hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox)
[download](https://hortonworks.com/downloads/#sandbox)
[install instruction](https://hortonworks.com/tutorial/sandbox-deployment-and-install-guide/section/3/)
[getting started](https://hortonworks.com/tutorial/hadoop-tutorial-getting-started-with-hdp/)

## Web SSH 
```
localhost:4200
root/hadoop
```

## SSH access
```
ssh root@localhost -p 2222
```

## setup after installation, init, ambari password reset
* shell web client (aka shell-in-a-box): 
localhost:4200 
root / hadoop
* ambari-admin-password-reset
* ambari-agent restart
* login into ambari:
localhost:8080
admin/{your password}

## Zeppelin UI
http://localhost:9995
user: maria_dev
pass: maria_dev

## install jupyter for spark
https://hortonworks.com/hadoop-tutorial/using-ipython-notebook-with-apache-spark/
```
PARK_MAJOR_VERSION is set to 2, using Spark2
Error in pyspark startup:
IPYTHON and IPYTHON_OPTS are removed in Spark 2.0+. Remove these from the environment and set PYSPARK_DRIVER_PYTHON and PYSPARK_DRIVER_PYTHON_OPTS instead.
just set variable to using Spart1 inside script: SPARK_MAJOR_VERSION=1
```

## Sqoop ( SQl to hadOOP )
import 
```
sqoop import --connect jdbc:mysql://127.0.0.1/crm?user=michael --table customers --target-dir /crm/users/michael.csv --as-textfile --fields-terminated-by ','
```


## HCatalog
[documentation](https://cwiki.apache.org/confluence/display/Hive/HCatalog)

### table description
```
hcat -e "describe school_explorer"
hcat -e "describe formatted school_explorer"
```

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


## SQL engines
- Impala
- Phoenix ( HBase )
- Drill ( schema-less sql )
- BigSQL ( PostgreSQL + Hadoop )
- Spark

## [Oozie](https://oozie.apache.org/)
workflow scheduler
```
START -> ACTION -> OK | ERROR
```

## Cascading
TBD

## Scalding
TBD


## Pig
free data structure ( Hive - fixed data structure )
execute into local mode
```
pig -x local
pig -x local <filename>
```
load data from file
```
LOAD <path to file> USING PigStorage(';') AS (userId: chararray, timestamp: long ); 
LOAD <path to file> AS (userId: chararray, timestamp: long ); # space will be used as delimiter 
```
[custom delimiter implementation](https://stackoverflow.com/questions/26354949/how-to-load-files-with-different-delimiter-each-time-in-piglatin#26356592)
[Pig UDF](https://pig.apache.org/docs/latest/udf.html)

save data
```
STORE <var name> INTO <path to file>
```
describe variable, print type
```
DESCRIBE <var name>;
```
print content of the variable
```
DUMP <var name>;
```
group into new variable
```
{bag} = GROUP <var name> by <field name>;
```
history of the variable, variable transformation steps
```
ILLUSTRATE <var name>;
```
map value one-by-one, walk through variable 
```
{bag} = FOREACH <var name> GENERATE <var field>, FUNCTION(<var field>);  
```
functions 
```
TOKENIZE - split
FLATTEN, - flat map
COUNT, 
SUM,....
```
filter by condition
```
FILTER <var name> BY <field name> operation;
FILTER <var name> BY <field name> MATCHES <regexp>;
```
join variables, inner join, outer join for variables
```
posts = LOAD '/data/user-posts.txt' USING PigStorage(',') AS (user:chararray, post:chararray, date:timestamp);
likes = LOAD '/data/user-likes.txt' USING PigStorage(',') AS (user_name:chararray, user_like:chararray, like_date:long);
JOIN posts BY user, likes BY user_name;
JOIN posts BY user LEFT OUTER, likes BY user_name;
JOIN posts BY user FULL OUTER, likes BY user_name;
> result will be: ( posts::user, posts::post, posts::date, likes:user_name, likes:user_like, likes:like_date )
```

## Spark
[app examples](https://mvnrepository.com/artifact/org.apache.spark/spark-examples)
[scala, java, python start point app](https://courses.cognitiveclass.ai/asset-v1:BigDataUniversity+BD0211EN+2016+type@asset+block/Exercise_3.pdf)

### packages
java/scala packages
```
<dependency>
    <groupId>org.apache.spark</groupId>
    <artifactId>spark-core_2.10</artifactId>
    <version>2.2.2</version>
</dependency>
```
```
org.apache.spark.SparkContext.*
org.apache.spark.SparkConf
org.apache.spark.api.java.JavaRDD
org.apache.spark.api.java.JavaSparkContext
```
python
```
from pyspark import SparkContext, SparkConf
```

### context
scala
```
val sparkConfig = new SparkConf().setAppName("my-app").setMaster("local[4]")
val sc = new SparkContext(  )
val sqlContext = new org.apache.spark.sql.SQLContext(sc)
val streamingContext = new StreamingContext(conf, Seconds(1))
```
java
```
sc = new JavaSparkContext( new SparkConf().setAppName("my-app").setMaster("local[*]") )

new org.apache.spark.sql.api.java.SQLContext(sc)
```
python
```
sc = SparkContext( conf = SparkConf().setAppName("my-app").setMaster("local") )

from pyspark.sql import SQLContext
sqlContext = SQLContext(sc)
```

### passing functions into Spark
* anonymous functions syntax
```
(a: Int) => a + 1
```
* static method
```
object FunctionCollections{
	def sizeCounter(s: String): Int = s.length
}
currentRdd.map(FunctionsCollections.sizeCounter)
```
* by reference
```
def myExecutor(rdd: RDD[String]): RDD[String] = {... rdd.map(a=> a+1) }
```

### libraries
* Spark SQL
```
val employee = sqlContext.sql("select employee.name, employee.age from employee where salary>120000")
employee.map(empl=> "Name:"+empl(0)+"  age:"+empl(1).sort(false).take(5).foreach(println)
```
* Spark Streaming
```
val lines = streamingContext.socketTextStream("127.0.0.1", 8888)
val words = lines.flatMap(_.split(" "))
words.map(w=>(w,1)) .reduceByKey(_ + _) .print()
```
* MLib
* GraphX
```
GraphLoader.edgeListFile(sc, path_to_file)
```


## Hadoop streaming. 
- Storm ( real time streaming solution )
- Spark ( near real time streaming, uses microbatching )
- Samza ( streaming on top of Kafka )
- Flink ( common approach to batch and stream code development )


## Security
- File permissions ( posix attributes )
- Hive ( grant revoke )
- Knox ( REST API for hadoop )
- Ranger 


## Data storage, NoSQL
### HBase
- distributed, column-oriented persistent multidimensional sorted map
- storing column-family into memory/disc
- disc = hdfs or filesystem
- column family has 'veracity' - version of the record based on timestamp
- Value = Table + RowKey + *Family* + Column + *Timestamp*

```
data is sparse - a lot of column has null values
fast retrieving data by 'key of the row' + 'column name'
contains from: (HBase HMaster) *---> (HBase Region Server)
```
### Accumulo
TBD


### Druid

## Cluster management
### [ambari](http://localhost:8080)

* cloudera manager

# TODO
download all slides from stepik - for repeating and creating xournals
