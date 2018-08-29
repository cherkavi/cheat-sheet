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

![record, logical view](https://s19.postimg.cc/bjssr74gz/hbase-record-logical-view.png)
[![hbase-acid.png](https://s19.postimg.cc/7ao2orlpf/hbase-acid.png)](https://postimg.cc/image/6xwoil3fj/)
[![hbase-record-logical-view.png](https://s19.postimg.cc/bjssr74gz/hbase-record-logical-view.png)](https://postimg.cc/image/t9uhc8i1r/)
[![hbase-record-phisical-view.png](https://s19.postimg.cc/rjbgafnkj/hbase-record-phisical-view.png)](https://postimg.cc/image/3sc2sbndb/)
[![hbase-why.png](https://s19.postimg.cc/43tj55w4j/hbase-why.png)](https://postimg.cc/image/bwk6x523j/)
[![hbase-logical-to-phisical-view.png](https://s19.postimg.cc/dpn3lfsf7/hbase-logical-to-phisical-view.png)](https://postimg.cc/image/vfos6h5zz/)
[![hbase-architecture.jpg](https://s19.postimg.cc/uq5zufaoz/hbase-architecture.jpg)](https://postimg.cc/image/g6yut0hjz/)
[![hbase-table-characteristics.png](https://s19.postimg.cc/jruqcabtv/hbase-table-characteristics.png)](https://postimg.cc/image/7pzci52lb/)
[![hbase-column-family.png](https://s19.postimg.cc/z1ulj2mn7/hbase-column-family.png)](https://postimg.cc/image/xmt0ucljz/)


start/stop hbase
$HBASE_HOME/bin/start-hbase.sh
$HBASE_HOME/bin/stop-hbase.sh

shell
$HBASE_HOME/bin/hbase shell

commands
* list of the tables
```
list
```

* create table 
```
create table 'mytable1'
```

* delete table
```
drop table 'mytable1'
disable table 'mytable1'
```

* iterate through a table
```
scan 'mytable1'
```

* insert data
```
put 'mytable1', 'row0015', 'cf:MyColumnFamily2', 'my value 01'
```

* read data
```
get 'mytable1', 'row0015'
```
