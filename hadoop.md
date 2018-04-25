# Hadoop

### [couldera container start](https://www.cloudera.com/documentation/enterprise/latest/topics/quickstart_docker_container.html#cloudera_docker_container)
docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 7180 4239cd2958c6 /usr/bin/docker-quickstart

### couldera run:
docker run -v /tmp:/home/root/tmp --net docker.local.network --ip 172.18.0.100 --hostname hadoop-local --network-alias hadoop-docker -t -i sequenceiq/hadoop-docker 

### help
./hdfs dfs -help
./hdfs dfs -help copyFromLocal

### list files
./hdfs dfs -ls /user/root/input
./hdfs dfs -ls hdfs://hadoop-local:9000/data
output example:
-rw-r--r--   1 root supergroup       5107 2017-10-27 12:57 hdfs://hadoop-local:9000/data/Iris.csv
             ^ factor of replication

### create folder
./hdfs dfs -mkdir /data 

### copy files from local filesystem to remote
./hdfs dfs -copyFromLocal /home/root/tmp/Iris.csv /data/

### file info ( disk usage )
./hdfs dfs -du -h

### file info ( disk usage )
./hdfs dfs -test 

### filesystem capacity
./hdfs dfs -df -h

### file system check, reporting
./hdfs fsck /

### balancer for distributed file system
./hdfs balancer

### administration of the filesystem
./hdfs dfsadmin -help


## Hortonworks sandbox
[tutorial.credentials](https://hortonworks.com/tutorial/learning-the-ropes-of-the-hortonworks-sandbox)

### Web SSH 
localhost:4200
root/hadoop

### SSH access
ssh root@localhost -p 2222


### ambari password reset
* shell web client (aka shell-in-a-box): 
localhost:4200 
root / hadoop
* ambari-admin-password-reset
* ambari-agent restart
* login into ambari:
localhost:8080
admin/{your password}


### Zeppelin UI
http://localhost:9995


