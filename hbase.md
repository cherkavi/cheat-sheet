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
