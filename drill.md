# Drill
## official doc
* [core modules](http://drill.apache.org/docs/core-modules/)

## start embedded
```
# install drill 
## https://drill.apache.org/download/
mkdir /home/projects/drill
cd /home/projects/drill
curl -L 'https://www.apache.org/dyn/closer.lua?filename=drill/drill-1.19.0/apache-drill-1.19.0.tar.gz&action=download' | tar -vxzf -
```
or 
```
sudo apt-get install default-jdk
curl -o apache-drill-1.6.0.tar.gz http://apache.mesi.com.ar/drill/drill-1.6.0/apache-drill-1.6.0.tar.gz
tar xvfz apache-drill-1.6.0.tar.gz
cd apache-drill-1.6.0
```

(skip for embedded ) create /home/projects/temp/drill-override.conf
```properties
drill.exec: {
  cluster-id: "drillbits1",
  zk.connect: "localhost:2181"
}
```

http://drill.apache.org/docs/s3-storage-plugin/
vim apache-drill-1.19.0/conf/core-site.xml
```xml
<configuration>
       <property>
           <name>fs.s3a.access.key</name>
           <value>AKIA6L...</value>
       </property>
       <property>
           <name>fs.s3a.secret.key</name>
           <value>nqGApjHh....</value>
       </property>
       <property>
           <name>fs.s3a.endpoint</name>
           <value>us-east-1</value>
       </property>
       <property>
           <name>fs.s3a.endpoint</name>
           <value>s3.REGION.amazonaws.com</value>
       </property>  
</configuration>  
```

```sh
# start drill locally
cd /home/projects/drill
# apache-drill-1.19.0/bin/sqlline -u jdbc:drill:zk=local
./apache-drill-1.19.0/bin/drill-embedded

Storage->S3->Update
```json
{
  "type": "file",
  "connection": "s3a://wond.../",
  "config": {
    "fs.s3a.secret.key": "nqGApjHh",
    "fs.s3a.access.key": "AKIA6LW",
    "fs.s3a.endpoint": "s3.us-east-1.amazonaws.com",
    "fs.s3a.impl.disable.cache":"true"
  },
```

## start docker
```sh
docker run -it --name drill-1.19.0 -p 8047:8047 -v /home/projects/temp/drill/conf:/opt/drill/conf --detach apache/drill:1.19.0 /bin/bash 
# docker ps -a | awk '{print $1}' | xargs docker rm {}
x-www-browser http://localhost:8047

```

## configuration after start
http://localhost:8047/storage > s3 > Update (check below) > Enable
```json
  "connection": "s3a://wonder-dir...",
  "config": {
    "fs.s3a.secret.key": "nqGApjHh2i...",
    "fs.s3a.access.key": "AKIA6LWYA...",
    "fs.s3a.endpoint": "us-east-1"
  },
```
should appear in "Enabled Storage Plugins"


## connect to existing on MapR 
```bash
# login
maprlogin password
echo $CLUSTER_PASSWORD | maprlogin password -user $CLUSTER_USER
export MAPR_TICKETFILE_LOCATION=$(maprlogin print | grep "keyfile" | awk '{print $3}')

# open drill
/opt/mapr/drill/drill-1.14.0/bin/sqlline -u "jdbc:drill:drillbit=ubs000103.vantagedp.com:31010;auth=MAPRSASL"
```
[drill shell](https://drill.apache.org/docs/configuring-the-drill-shell/)
```sh
# start recording console to file, write output
!record out.txt
# stop recording
record
```

### drill querying data 
```sql
show databases; -- show schemas;
select sessionId, isReprocessable from dfs.`/mapr/dp.prod.zurich/vantage/data/store/processed/0171eabfceff/reprocessable/part-00000-63dbcc0d1bed-c000.snappy.parquet`;
-- or even 
select sessionId, isReprocessable from dfs.`/mapr/dp.prod.zurich/vantage/data/store/processed/*/*/part-00000-63dbcc0d1bed-c000.snappy.parquet`;
-- with functions
to_char(to_timestamp(my_column), 'yyyy-MM-dd HH:mm:ss')
to_number(concat('0', mycolumn),'#')
```
!!! important: you should avoid colon ':' symbol in path ( explicitly or implicitly with asterix )

[drill http](https://docs.datafabric.hpe.com/61/Drill/drill-web-permissions.html)
```sh
# check status
curl --insecure -X GET https://mapr-web.vantage.zur:21103/status

# obtain cookie from server
curl -H "Content-Type: application/x-www-form-urlencoded" \
  -k -c cookies.txt -s \
  -d "j_username=$DRILL_USER" \
  --data-urlencode "j_password=$DRILL_PASS" \
  https://mapr-web.vantage.zur:21103/j_security_check

# obtain version
curl -k -b cookies.txt -X POST \
-H "Content-Type: application/json" \
-w "response-code: %{http_code}\n" \
-d '{"queryType":"SQL", "query": "select * from sys.version"}' \
https://mapr-web.vantage.zur:21103/query.json


# SQL request
curl -k -b cookies.txt -X POST \
-H "Content-Type: application/json" \
-w "response-code: %{http_code}\n" \
-d '{"queryType":"SQL", "query":  "select loggerTimestamp, key, `value` from dfs.`/mapr/dp.zurich/some-file-on-cluster` limit 10"}' \
https://mapr-web.vantage.zur:21103/query.json
```

## drill java
[src code](https://github.com/cherkavi/java-code-example/blob/master/drill/src/main/java/drill/DrillCollaboration.java)
```sh
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.222/jre/bin/java \
-Ddrill.customAuthFactories=org.apache.drill.exec.rpc.security.maprsasl.MapRSaslFactory \
-Djava.security.auth.login.config=/opt/mapr/conf/mapr.login.conf \
-Dzookeeper.sasl.client=false \
-Dlog.path=/opt/mapr/drill/drill-1.14.0/logs/sqlline.log \
-Dlog.query.path=/opt/mapr/drill/drill-1.14.0/logs/sqlline_queries/data_api-s_sqlline_queries.json \
-cp /opt/mapr/drill/drill-1.14.0/conf:/opt/mapr/drill/drill-1.14.0/jars/*:/opt/mapr/drill/drill-1.14.0/jars/ext/*:/opt/mapr/drill/drill-1.14.0/jars/3rdparty/*:/opt/mapr/drill/drill-1.14.0/jars/classb/*:/opt/mapr/drill/drill-1.14.0/jars/3rdparty/linux/*:drill_jdbc-1.0-SNAPSHOT.jar \
DrillCollaboration
```

## drill special commands
increasing amount of parallel processing threads
```sql
set planner.width.max_per_node=10;
```

show errors in log
```sql
ALTER SESSION SET `exec.errors.verbose`=true;
```

## sql examples
```sql
select columns[0], columns[1] from s3.`sample.csv`
```
