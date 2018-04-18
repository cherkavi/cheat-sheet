# Hadoop 

## Hadoop into Docker container 

### [couldera container start](https://www.cloudera.com/documentation/enterprise/latest/topics/quickstart_docker_container.html#cloudera_docker_container)
> docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 7180 4239cd2958c6 /usr/bin/docker-quickstart

### couldera run:
> docker run -v /tmp:/home/root/tmp --net docker.local.network --ip 172.18.0.100 --hostname hadoop-local --network-alias hadoop-docker -t -i sequenceiq/hadoop-docker 


## HDFS common commands

### help ( Distributed File System )
> hdfs dfs -help
> hdfs dfs -help copyFromLocal
> hdfs dfs -help ls
> hdfs dfs -help cat
> hdfs dfs -help setrep


### list files
> hdfs dfs -ls /user/root/input
> hdfs dfs -ls hdfs://hadoop-local:9000/data
output example:
-rw-r--r--   1 root supergroup       5107 2017-10-27 12:57 hdfs://hadoop-local:9000/data/Iris.csv
             ^ factor of replication

### change factor of replication 
> hdfs dfs -setrep -w 4 /data/file.txt

### create folder
> hdfs dfs -mkdir /data 

### copy files from local filesystem to remote
> hdfs dfs -put /home/root/tmp/Iris.csv /data/
> hdfs dfs -copyFromLocal /home/root/tmp/Iris.csv /data/

### copy files from local filesystem to remote with replication factor
> hdfs dfs -Ddfs.replication=2 -put /path/to/local/file /path/to/hdfs

### copy ( small files only !!! ) from local to remote ( read from DataNodes and write to DataNodes !!!)
> hdfs dfs -cp /home/root/tmp/Iris.csv /data/

### remote copy ( not used client as pipe )
> hdfs distcp /home/root/tmp/Iris.csv /data/

### read data from DataNode
> hdfs get /path/to/hdfs /path/to/local/file
> hdfs dfs -copyToLocal /path/to/hdfs /path/to/local/file

### remove data from HDFS ( to Trash !!! special for each user)
> hdfs rm -r /path/to/hdfs-folder

### remove data from HDFS
> hdfs rm -r -skipTrash /path/to/hdfs-folder

### file info ( disk usage )
> hdfs dfs -du -h /path/to/hdfs-folder

### is file/folder exists ? 
> hdfs dfs -test /path/to/hdfs-folder

### list of files ( / - root )
> hdfs dfs -ls /
> hdfs dfs -ls hdfs://192.168.1.10:8020/path/to/folder
the same as previous but with fs.defalut.name = hdfs://192.168.1.10:8020
> hdfs dfs -ls /path/to/folder
> hdfs dfs -ls file:///local/path   ==   (ls /local/path)
show all sub-folders
> hdfs dfs -ls -r 

### standard command for hdsf
-cat (-text), -tail, -mkdir, -chmod, - chown, count, test ....



## Hadoop governance, administration

### filesystem capacity
> hdfs dfs -df -h

### file system check, reporting, file system information
> hdfs fsck /

### balancer for distributed file system
> hdfs balancer

### administration of the filesystem
> hdfs dfsadmin -help

