# HBase
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

* insert data
```
put 'mytable1', 'row0015', 'cf:MyColumnFamily2', 'my value 01'
```

* read data
```
get 'mytable1', 'row0015'
```
