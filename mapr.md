[MapR Academy](http://learn.mapr.com)

# MapR Streams
## parallelism
* Partition Id
* Hash of messageId
* Round-Robin 

## sending messages via client library
![sending by client](https://i.postimg.cc/0NKdXfhm/mapr-streams-sending-01.png)
![consuming by broker](https://i.postimg.cc/wvK5CNdr/mapr-streams-sending-02.png)

## replication
![replication](https://i.postimg.cc/QtbBfKN1/Mapr-streams-replication-01.png)
![back loop](https://i.postimg.cc/tJ4swR7f/Mapr-streams-replication-02.png)

## reading messages via client library
![lib request](https://i.postimg.cc/23pfv2pw/Mapr-strems-reading-01.png)
![client rading](https://i.postimg.cc/SQn26RF2/Mapr-strems-reading-02.png)

## reading messages cursor types
* Read cursor ( client request it and broker sent )
* Committed cursor ( client confirmed/commited reading )
![cursor types](https://i.postimg.cc/s28cc8X5/Mapr-streams-cursor-types.png)

## Replicating streams
* Master->Slave 
* Many->One
* MultiMaster: Master<-->Master
* Stream replications: Node-->Node2-->Node3-->Node4 ... ( with loop preventing )

## command line
### create stream 
```
maprcli stream create -path <filepath & name>
maprcli stream create -path <filepath & name> -consumeperm u:<userId> -produceperm u:<userId> -topicperm u:<userId>
```
### create topic
```
maprcli stream topic create -path <path and name of the stream> -topic <name of the topic>
```
