# Big Data 
* [mapr](./mapr-cheat-sheet.md)
* [hadoop](./hadoop-cheat-sheet.md)
* [hbase](./hbase-cheat-sheet.md)
* [helm](./helm-cheat-sheet.md)
* [hive](./hive-cheat-sheet.md)
* [pig](./pig-cheat-sheet.md)
* [nosql](./nosql.md)
* [spark](./spark-cheat-sheet.md)
* [airflow](./airflow-cheat-sheet.md)
* [bigsql](./bigsql-cheat-sheet.md)
* [cassandra](./cassandra-cheat-sheet.md)


## DataDriven Culture

```mermaid
flowchart RL

DC[Data
   Catalog]  --o Fabric
MD[MetaData] --o Fabric 
```

```mermaid
graph RL

d1[domain business unit] --o DM[Data Mesh]
d2[data as product] --o DM
d3[self service] --o DM
d4[federal governance] --o DM

BD[Business
   Driven Requirements] --influent--> DM    

```