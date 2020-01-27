# Apache Cassandra
* one of the Base Availability Soft_state Eventually_consistent storage   
* using CassandraQueryLanguage for querying data  
* no master, managed by background daemon process

## levels of data
* KeySpace
* ColumnFamily
* Column
* Key

## Columns
     FamilyColumn <>---------- Column
SuperFamilyColumn <>----- SuperColumn
CompositeColumn

## Reading modes:
* one: from first node
* quorum: when more than 50% of nodes responded
* all ( consistent ): when all nodes answered

## Writing modes:
* zero: no wait, just send data and close connection by client
* any: at least one node should answered
* one: node answered, commit log was written, new value placed in memory
* quorum: more than 50% confirmed
* all: all nodes confirmed writing

