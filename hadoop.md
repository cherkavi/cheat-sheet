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

### IBM education container start
```
docker run -it --name bdu_spark2 -P -p 4040:4040 -p 4041:4041 -p 8080:8080 -p8081:8081 bigdatauniversity/spark2:latest
-- /etc/bootstrap.sh -bash 
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

### Security
- File permissions ( posix attributes )
- Hive ( grant revoke )
- Knox ( REST API for hadoop )
- Ranger 


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

# Sqoop ( SQl to/from hadOOP )
JDBC driver for jdbc url must present: $SQOOP_HOME/lib
--- 

### import
Import destinations:
* text files
* binary files
* HBase
* Hive
```
sqoop import --connect jdbc:mysql://127.0.0.1/crm --username myuser --password mypassword --table customers --target-dir /crm/users/michael.csv  
```
additional parameter to leverage amount of mappers that working in parallel:
```
--split-by customer_id_pk
```
additional parameters:
```
--fields-terminated-by ','
--columns "name, age, address"
--where "age>30"
--query "select name, age, address from customers where age>30"
```
additional import parameters:
```
--as-textfile
--as-sequencefile
--as-avrodatafile
```

### export
export modes:
* insert
```
sqoop export --connect jdbc:mysql://127.0.0.1/crm --username myuser --password mypassword --export-dir /crm/users/michael.csv --table customers 
```
* update
```
sqoop export --connect jdbc:mysql://127.0.0.1/crm --username myuser --password mypassword --export-dir /crm/users/michael.csv --udpate_key user_id
```
* call ( store procedure will be executed )
```
sqoop export --connect jdbc:mysql://127.0.0.1/crm --username myuser --password mypassword --export-dir /crm/users/michael.csv --call customer_load
```
additional export parameters:
```
# row for a single insert
-Dsqoop.export.records.per.statement
# number of insert before commit
-Dexport.statements.per.transaction
```

---
# HCatalog
[documentation](https://cwiki.apache.org/confluence/display/Hive/HCatalog)

### table description
```
hcat -e "describe school_explorer"
hcat -e "describe formatted school_explorer"
```
---
# SQL engines
- Impala
- Phoenix ( HBase )
- Drill ( schema-less sql )
- BigSQL ( PostgreSQL + Hadoop )
- Spark

# [Oozie](https://oozie.apache.org/)
workflow scheduler
```
START -> ACTION -> OK | ERROR
```

# Cascading
TBD

# Scalding
TBD


--- 
# Hadoop streaming. 
- Storm ( real time streaming solution )
- Spark ( near real time streaming, uses microbatching )
- Samza ( streaming on top of Kafka )
- Flink ( common approach to batch and stream code development )


---
# Data storage, NoSQL
## Accumulo
TBD

## Druid
TBD

# Cluster management
## [ambari](http://localhost:8080)
* cloudera manager


---
# TODO
download all slides from stepik - for repeating and creating xournals
