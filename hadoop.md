# Hadoop 
* [tutorial](http://hadooptutorial.info)
* [MapR documentation](https://maprdocs.mapr.com/51/WR-ecosystem-intro.html)
* [Cloudera documentation](https://www.cloudera.com/documentation/enterprise/latest.html)
* [Hortonworks tutorials](https://hortonworks.com/tutorials/)
* [Hortonworks ecosystems](https://hortonworks.com/ecosystems/)

## Theory
![what data is](https://i.ibb.co/Zg26wmq/2023-08-27-data-what-is-it.jpg)  

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

![hadoop2 yarn](https://i.postimg.cc/C522hxhG/Hadoop2-Yarn.png)
![hadoop1](https://i.postimg.cc/zG2F4cfh/hdfs-map-R.png)
![sensor use case](https://i.postimg.cc/BvzTXRNH/Kafka-usecase-01.png)
![sensor use case](https://i.postimg.cc/QCGcq52M/Kafka-usecase-02.png)
![sensor use case](https://i.postimg.cc/Kz9dkGwQ/Sensor-usecase-03.png)
![sensor use case](https://i.postimg.cc/mgyRZsLp/Sensor-usecase-04.png)
![sensor use case](https://i.postimg.cc/jdRM001S/Kafka-usecase-05.png)
![sensor use case](https://i.postimg.cc/8PwtTX8B/Kafka-usecase-06.png)

## HDFS common commands
### MapR implementation
```sh
# hdfs dfs -ls
hadoop fs -ls
```

### admin command, cluster settings
```
hdfs dfsadmin -report
```

### list of namenodes, list of secondary nodes
```
hdfs getconf -namenodes
hdfs getconf -secondaryNameNodes
hdfs getconf -confKey dfs.namenode.name.dir
```
confKey:
* dfs.namenode.name.dir
* fs.defaultFS
* yarn.resourcemanager.address
* mapreduce.framework.name
* dfs.namenode.name.dir
* dfs.default.chunk.view.size
* dfs.namenode.fs-limits.max-blocks-per-file
* dfs.permissions.enabled
* dfs.namenode.acls.enabled
* dfs.replication
* dfs.replication.max
* dfs.namenode.replication.min
* dfs.blocksize
* dfs.client.block.write.retries
* dfs.hosts.exclude
* dfs.namenode.checkpoint.edits.dir
* dfs.image.compress
* dfs.image.compression.codec
* dfs.user.home.dir.prefix
* dfs.permissions.enabled
* io.file.buffer.size
* io.bytes-per-checksum
* io.seqfile.local.dir

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
### files count
```
hdfs dfs -count /user/root/input
```
where 1-st column - amount of folder ( +1 current ),
where 2-nd column - amount of files into folder
where 3-rd column - size of folder

### check if folder exists
```
hdfs dfs -test -d /user/root/some_folder
echo $?
```
0 - exists
1 - not exits

### checksum ( md5sum )
```
hdfs dfs -checksum <path to file>
```
[hdfs logic emulator](https://github.com/srch07/HDFSChecksumForLocalfile)
```
java -jar HadoopChecksumForLocalfile-1.0.jar V717777_MDF4_20190201.MF4 0 512 CRC32C
```
locally only
```
hdfs dfs -cat <path to file> | md5sum
```

### find folders ( for cloudera only !!! )
```
hadoop jar /opt/cloudera/parcels/CDH/jars/search-mr-1.0.0-cdh5.14.4-job.jar org.apache.solr.hadoop.HdfsFindTool -find hdfs:///data/ingest/ -type d -name "some-name-of-the-directory"
```

### find files ( for cloudera only !!! )
```
hadoop jar /opt/cloudera/parcels/CDH/jars/search-mr-1.0.0-cdh5.14.4-job.jar org.apache.solr.hadoop.HdfsFindTool -find hdfs:///data/ingest/ -type f -name "some-name-of-the-file"
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
hadoop distcp /home/root/tmp/Iris.csv /data/
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
### java application run, java run, java build
```
hadoop classpath
hadoop classpath glob
# javac -classpath `hadoop classpath` MyProducer.java
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
### application list on YARN
```
yarn application --list
```
### application list with ALL states
```
yarn application -list -appStates ALL
```
### application status
```
yarn application -status application_1555573258694_20981
```
### application kill on YARN
```
yarn application -kill application_1540813402987_3657
```
### application log on YARN
```
yarn logs -applicationId application_1540813402987_3657 | less
```
### application log on YARN by user
```
yarn logs -applicationId application_1540813402987_3657 -appOwner my_tech_user | less
```

#### YARN REST API
```sh
# https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/ResourceManagerRest.html#Cluster_Writeable_APIs
# https://docs.cloudera.com/runtime/7.2.1/yarn-reference/topics/yarn-use-the-yarn-rest-apis-to-manage-applications.html
YARN_USER=$USER_AD
YARN_PASS=$USER_AD
YARN_URL=https://mapr-web.vantage.zur
YARN_PORT=10101

APP_ID=application_1670952222247_1113333
# application state 
curl --user $YARN_USER:$YARN_PASS --insecure $YARN_URL:$YARN_PORT/ws/v1/cluster/apps/$APP_ID/state
# kill application 
curl -v -X PUT -d '{"state": "KILLED"}' $YARN_URL:$YARN_PORT/ws/v1/cluster/apps/$APP_ID

# application find 
APP_TAG=33344447eb9c81f4fd7a
curl -X GET --user $YARN_USER:$YARN_PASS --insecure $YARN_URL:$YARN_PORT/ws/v1/cluster/apps?applicationTags=$APP_TAG | jq .
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
### java application run, java run, java build
```
mapr classpath
mapr classpath glob
```

### compile java app, execute java app
```
javac -classpath `mapr classpath` MyProducer.java
java -classpath `mapr classpath`:. MyProducer
```

---
# MDF4 reading
```python
header = mdfreader.mdfinfo4.Info4("file.MF4")
header.keys()
header['AT'].keys()
header['AT'][768]['at_embedded_data']
```
```python
info=mdfreader.mdfinfo()
info.listChannels("file.MF4")
```
```python
from asammdf import MDF4 as MDF
mdf = MDF("file.MF4")
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
