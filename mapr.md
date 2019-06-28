[MapR Academy](http://learn.mapr.com)
[Sandbox](https://mapr.com/docs/home/SandboxHadoop/c_sandbox_overview.html)

# Architecture examples
![connected drive](https://i.postimg.cc/LXCBm8b5/Connected-Car-Pipeline.png)

# MapR Streams
## parallelism
* Partition Id
* Hash of messageId
* Round-Robin 

## sending messages via client library
![sending by client](https://i.postimg.cc/0NKdXfhm/mapr-streams-sending-01.png)
![consuming by broker](https://i.postimg.cc/wvK5CNdr/mapr-streams-sending-02.png)

## spreading message between partitions, assigning message to paritiion
* by partition number
* by message key
* round-robin ( without previous two )
* properties.put("streams.patitioner.class", "my.package.MyClassName.class")
```
public class MyClassName implements Partitioner{
   public int partition( String topic, Object, key, byte[] keyBytes, Object value, byte[] valueBytes, Cluster cluster){}
}
```


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
### find CLDB hosts ( ContainerLocationDataBase )
```
maprcli node listcldbs
```

### stream create
```
maprcli stream create -path <filepath & name>
maprcli stream create -path <filepath & name> -consumeperm u:<userId> -produceperm u:<userId> -topicperm u:<userId>
maprcli stream create -path <filepath & name> -consumeperm "u:<userId>" -produceperm "u:<userId>" -topicperm "u:<userId>" -adminperm "u:<userId1> | u:<userId2>"
```
### stream remove, stream delete
```
maprcli stream delete -path <filepath & name>
```

### topic create
```
maprcli stream topic create -path <path and name of the stream> -topic <name of the topic>
```
### topic remove, topic delete
```
maprcli stream topic delete -path <path and name of the stream> -topic <name of the topic>
```
### topic check, topic print
```
maprcli stream topic list -path <path and name of the stream>
```

## API, java programming
### producer
#### 
```
java -classpath kafka-clients-1.1.1-mapr-1808.jar:slf4j-api-1.7.12.jar:slf4j-log4j12-1.7.12.jar:log4j-1.2.17.jar:mapr-streams-6.1.0-mapr.jar:maprfs-6.1.0-mapr.jar:protobuf-java-2.5.0.jar:hadoop-common-2.7.0.jar:commons-logging-1.1.3-api.jar:commons-logging-1.1.3.jar:guava-14.0.1.jar:commons-collections-3.2.2.jar:hadoop-auth-2.7.0-mapr-1808.jar:commons-configuration-1.6.jar:commons-lang-2.6.jar:jackson-core-2.9.5.jar:. MyConsumer
```
#### java example, kafka java application
```
Properties properties = new Properties();
properties.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
properties.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
// org.apache.kafka.common.serialization.ByteSerializer
// properties.put("client.id", <client id>)

import org.apache.kafka.clients.producer.KafkaProducer;
KafkaProducer producer = new KafkaProducer<String, String>(properties);

String streamTopic = "<streamname>:<topicname>"; // "/streams/my-stream:topic-name"
ProducerRecord<String, String> record = new ProducerRecord<String, String>(streamTopic, textOfMessage);
// ProducerRecord<String, String> record = new ProducerRecord<String, String>(streamTopic, messageTextKey, textOfMessage);
// ProducerRecord<String, String> record = new ProducerRecord<String, String>(streamTopic, partitionIntNumber, textOfMessage);

Callback callback = new Callback(){
  public void onCompletion(RecordMetadata meta, Exception ex){
    meta.offset();
  }
};
producer.send(record, callback);
producer.close();
```

#### sending conditions
![flash client buffer](https://i.postimg.cc/y8X75Z6P/Selection-009.png)

#### parallel sending
```
streams.parallel.flushers.per.partition default true:
```
* does not wait for ACK before sending more messages
* possible for messages to arrive out of order
```
streams.parallel.flushers.per.partition set to false: 
```
* client library will wait for ACK from server
* slower than default setting
![sending types](https://i.postimg.cc/c1w1Y2q2/Selection-010.png)

#### retrieving metadata during connection with Kafka
```
metadata.max.age.ms
```
How frequently to fetch metadata

### consumer
#### java consumer
```
Properties properties = new Properties();
properties.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
properties.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
// org.apache.kafka.common.serialization.ByteSerializer
// properties.put("auto.offset.reset", <Earliest, Latest, None>)
// properties.put("group.id", <group identificator>)
// properties.put("enable.auto.commit", <true - default | false >), use consumer.commitSync() if false
// properties.put("auto.commit.interval.ms", <default value 1000ms>)

import org.apache.kafka.clients.consumer.KafkaConsumer;
KafkaConsumer consumer = new KafkaConsumer<String, String>(properties);

String streamTopic = "<streamname>:<topicname>"; // "/streams/my-stream:topic-name"
consumer.subscribe(Arrays.asList(topic));
// consumer.subscribe(topic, new RebalanceListener());
ConsumerRecords<String, String> messages = consumer.poll(1000L); // reading with timeout
messages.iterator().next().toString(); // "/streams/my-stream:topic-name, parition=1, offset=256, key=one, value=text"
```

#### java rebalance listener
```
public class RebalanceListener implements ConsumerRebalanceListener{
    onPartitionAssigned(Collection<TopicPartition> partitions)
    onPartitionRevoked(Collection<TopicPartition> partitions)
}
```

### execute java app
(maven repository)[https://repository.mapr.com/nexus/content/repositories/releases/]
```
<repositories>
  <repository>
    <id>mapr-maven</id>
    <url>http://repository.mapr.com/maven</url>
    <releases>
      <enabled>true</enabled>
    </releases>
    <snapshots>
      <enabled>false</enabled>
    </snapshots>
  </repository>
</repositories>
<dependencies>
  <dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka-clients</artifactId>
    <version>0.9.0.0-mapr-1602-streams-5.1.0</version>
    <scope>provided</scope>
  </dependency>
</dependencies>
```
execute on cluster
```
mapr classpath
java -cp `mapr classpath`:my-own-app.jar mypackage.MainClass

```

# maprcli
## login, print info, logout
```
maprlogin password -user {your cluster username}
maprlogin print
maprlogin logout
```

## check your credential, expiration date/time
```
maprlogin print -ticketfile <your ticketfile> 
# you will see expiration date like 
# on 07.05.2019 13:56:47 created = 'Tue Apr 23 13:56:47 UTC 2019', expires = 'Tue May 07 13:56:47 UTC 2019'
```
