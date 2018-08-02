# main concepts
* Topics
category of messages, consists from Partitions
* Partition
part of the Topic, can be replicated (replication factor) across Brokers, must have at least one Leader and 0..* Followers
* Leader
main partition in certain period of time, contains InSyncReplica's - list of Followers that are alive in current time
* Committed Messages
when all InSyncReplicas wrote message, Consumer can read it after, Producer can wait for it or not
* Brokers
one of the server of Kafka ( one of the server of cluster )
* Producers
some process that publish message into specific topic
* Consumers
topics subscriber
* Consumer Group
group of consumers, have one Load Balancer for one group, 
consumer instance from different group will receive own copy of message ( one message per group )


# ZoopKeeper ( one instance per cluster )
* must be started before using Kafka ( zookeeper-server-start.sh, kafka-server-start.sh )
* cluster membership
* electing a controller
* topic configuration
leader, which topic exists
* Quotas
* ACLs

# Kafka guarantees
* messages that sent into particular topic will be appended in the same order
* consumer see messages in order that were written
* "At-least-once" message delivery guaranteed - for consumer who crushed before it commited offset
* "At-most-once" delivery - ( custom realization ) consumer will never read the same message again, even when crushed before process it


# scripts
## start Kafka's Broker
```
zookeeper-server-start.sh
kafka-server-start.sh config/server.properties
```

## topic create
```
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic mytopic
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --describe --topic mytopic
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --config retention.ms=360000 --topic mytopic
```
or just enable "autocreation"
```
auto.create.topics.enable=true
```

## topic delete
can be marked "for deletion"
```
bin/kafka-topics.sh --delete --zookeeper localhost:2181 --topic mytopic
```

## topics list

```
bin/kafka-topics.sh --create --zookeeper localhost:2181 --list
```

## topic update
```
bin/kafka-topics.sh --alter --zookeeper localhost:2181 --partitions 5 --topic mytopic
bin/kafka-topics.sh --alter --zookeeper localhost:2181 --topic mytopic --config retention.ms=72000
bin/kafka-topics.sh --alter --zookeeper localhost:2181 --topic mytopic --deleteConfig retention.ms=72000
```
