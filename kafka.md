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

