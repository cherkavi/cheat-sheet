# HBase
* distributed, 
* column-oriented persistent multidimensional sorted map
* autoscale
* storing column-family into memory/disc
* disc = hdfs or filesystem
* column familty (limited number) can be configured to:
  * compress
  * versions count
  * TimeToLive 'veracity'
  * in memory/on disc
  * separate file
* key - is byte[], value is byte[]
* scan by key
* scan by keys range
* schema free
* each record has a version
* TableName - filename
* record looks like: RowKey ; *ColumnFamilyName* ; ColumnName ; *Timestamp*
![record](https://i.postimg.cc/HL2PqYnD/hbase-record.png)  
![record-view](https://i.postimg.cc/cCQkWY1D/hBase.png)
* table can divide into number of regions (sorted be key with start...end keys and controlled by HMaster ) 
* region has default size 256Mb
```
data is sparse - a lot of column has null values
fast retrieving data by 'key of the row' + 'column name'
contains from: (HBase HMaster) *---> (HBase Region Server)
```
SQL for Hbase - [Phoenix SQL](https://phoenix.apache.org/)

## Why HBase
![hbase-why.png](https://s19.postimg.cc/43tj55w4j/hbase-why.png)

## HBase Architecture
![hbase-architecture.jpg](https://s19.postimg.cc/uq5zufaoz/hbase-architecture.jpg)

## HBase ACID
![hbase-acid.png](https://s19.postimg.cc/7ao2orlpf/hbase-acid.png)

## logical view
![hbase-record-logical-view.png](https://s19.postimg.cc/bjssr74gz/hbase-record-logical-view.png)

## phisical view
![hbase-record-phisical-view.png](https://s19.postimg.cc/rjbgafnkj/hbase-record-phisical-view.png)

## logical to phisical view
![hbase-logical-to-phisical-view.png](https://s19.postimg.cc/dpn3lfsf7/hbase-logical-to-phisical-view.png)

## Table characteristics
![hbase-table-characteristics.png](https://s19.postimg.cc/jruqcabtv/hbase-table-characteristics.png)

## Column-Family
![hbase-column-family.png](https://s19.postimg.cc/z1ulj2mn7/hbase-column-family.png)

## manage HBase
start/stop hbase
```
$HBASE_HOME/bin/start-hbase.sh
$HBASE_HOME/bin/stop-hbase.sh
```

## interactive shell
[cheat sheet](https://learnhbase.wordpress.com/2013/03/02/hbase-shell-commands/)
```
$HBASE_HOME/bin/hbase shell
```

## path to jars from hbase classpath
```
hbase classpath
```

## commands
* list of the tables
```
list
```

* create table 
```
create table 'mytable1'
```

* description of the table
```
descibe 'my_space:mytable1'
```

* count records
```
count 'my_space:mytable1'
```

* delete table
```
drop table 'mytable1'
disable table 'mytable1'
```

* iterate through a table, iterate with range
```
scan 'my_space:mytable1'
scan 'my_space:mytable1', {STARTROW=>"00223cfd-8b50-979d29164e72:1220", STOPROW=>"00223cfd-8b50-979d29164e72:1520"}
```
* save results into a file
```
echo " scan 'my_space:mytable1', {STARTROW=>"00223cfd-8b50-979d29164e72:1220", STOPROW=>"00223cfd-8b50-979d29164e72:1520"} " | hbase shell > out.txt
```

* insert data, update data
```
put 'mytable1', 'row0015', 'cf:MyColumnFamily2', 'my value 01'
```

* read data
```
get 'mytable1', 'row0015'
```

# Java
* [java examples from google](https://cloud.google.com/bigtable/docs/samples-hbase-java-hello)
## java app 
```
java \
    -cp /opt/cloudera/parcels/SPARK2/lib/spark2/jars/*:`hbase classpath`:{{ deploy_dir }}/lib/ingest-pipeline-orchestrator-jar-with-dependencies.jar \
    -Djava.security.auth.login.config={{ deploy_dir }}/res/deploy.jaas \
    com.bmw.ad.ingest.pipeline.orchestrator.admin.TruncateSessionEntriesHelper \
    --hbase-zookeeper {{ hbase_zookeeper }} \
    --ingest-tracking-table-name {{ ingest_tracking_table }} \
    --file-meta-table-name {{ file_meta_table }} \
    --component-state-table-name {{ component_state_table }} \
    --session-id $1
```
## [scan values](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/client/Scan.html)
```java
  Scan scan = new Scan(Bytes.toBytes(startAndStopRow), Bytes.toBytes(startAndStopRow));
  scan.addColumn(FAMILY_NAME, COLUMN_NAME);
  // scan.setFilter(new FirstKeyOnlyFilter());
  ResultScanner scanner = this.table.getScanner(scan);
  try{
   for ( Result result : scanner) { // each next() call - RPC call to server
     System.out.println(result);
   }
  }finally{
   scanner.close(); // !!! IMPORTANT !!!
  }
}
```

## get value
```java
Get record = new Get(Bytes.toBytes("row_key"));
record.addColumn(bytes.toBytes("column_family"), bytes.toBytes("column_name"));
Result result = mytable1.get(record);
# or
byte[] value = result.getValue(Bytes.toBytes("column_family"),Bytes.toBytes("column_name"))
```


## put record
```java
Put row=new Put(Bytes.toBytes("rowKey"));
row.add(Bytes.toBytes("column_family"), Bytes.toBytes("column"), Bytes.toBytes("value1"));
table.put(row);
```
```java
package com.learn.hbase.client;

import java.io.IOException;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.TableName;
import org.apache.hadoop.hbase.client.Connection;
import org.apache.hadoop.hbase.client.ConnectionFactory;
import org.apache.hadoop.hbase.client.Put;
import org.apache.hadoop.hbase.client.Table;
import org.apache.hadoop.hbase.util.Bytes;

public class PutHBaseClient {

public static void main(String[] args) throws IOException {

  Configuration conf = HBaseConfiguration.create();

  Connection connection = ConnectionFactory.createConnection(conf);
  Table table = connection.getTable(TableName.valueOf("test"));
  try {

    Put put1 = new Put(Bytes.toBytes("row1"));

    put1.addColumn(Bytes.toBytes("cf"), Bytes.toBytes("qual1"), Bytes.toBytes("ValueOneForPut1Qual1"));
    put1.addColumn(Bytes.toBytes("cf"), Bytes.toBytes("qual2"), Bytes.toBytes("ValueOneForPut1Qual2"));

    table.put(put1);

  } finally {
    table.close();
    connection.close();
  }

}

}
```

## update record
* checkAndPut - compares the value with the current value from the hbase according to the passed CompareOp. CompareOp=EQUALS Adds the value to the put object if expected value is equal.  
* checkAndMutate - compares the value with the current value from the hbase according to the passed CompareOp.CompareOp=EQUALS Adds the value to the rowmutation object if expected value is equal.  
row mutation example
```java 
RowMutations mutations = new RowMutations(row);
//add new columns
Put put = new Put(row);
put.add(cf, col1, v1);
put.add(cf, col2, v2);

Delete delete = new Delete(row);
delete.deleteFamily(cf1, now);

//delete column family and add new columns to same family
mutations.add(delete);
mutations.add(put);

table.mutateRow(mutations);
```

## delete value
```java
Delete row=new Delete(Bytes.toBytes("rowKey"));
row.deleteColumn(Bytes.toBytes("column_family"), Bytes.toBytes("column"));
table.delete(row);
# or
table.deleteColumn(Bytes.toBytes("column_family"), Bytes.toBytes("column"), timestamp)
table.deleteColumns(Bytes.toBytes("column_family"), Bytes.toBytes("column"), timestamp)
table.deleteFamily(Bytes.toBytes("column_family"))
```

## batch operation
```java
Put put = 
Get get = 

Object[] results = new Object[2];
table.batch(List.of(put, get), results);
```

## create table
```java
Configuration config = HbaseConfiguration.create();

HBaseAdmin admin = new HbaseAdmin(config);

HTableDescriptor tableDescriptor = new HTableDescriptor(Bytes.toBytes("my_table1"));
HColumnDescriptor columns = new HColumnDescriptor(Bytes.toBytes("column_family_1"));
tableDescriptor.addFamily(columns);
admin.createTable(tableDescriptor);
admin.isTableAvailable(Bytes.toBytes("my_table1"));
```
