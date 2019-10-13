REmote DIctionary Server

* [commands](https://redis.io/commands)
* [data types](https://redis.io/topics/data-types-intro)
* [REdis Serialization Protocol - RESP](https://redis.io/topics/protocol)  
* [pipeline](https://redis.io/topics/pipelining)
> pipeline == batch commands, will return result ONLY when ALL commands will be finished
* [transactions](https://redis.io/topics/transactions)
> transaction (multi, exec ) is like pipeline (pipelined, sync)
* [LUA scripting in Redis](https://redis.io/commands/eval), [lua playground](https://www.lua.org/cgi-bin/demo)
> like transaction will execute everything atomically
* no native indexes
  * use "secondary index" - build "inverted index"
  * use "faceted search" - build intersection of "inverted indexes" to find target search criteria
  * use "hashed search" - build set with hash-key ( long name of relations between data cardinality )

## commands
* DBSIZE
* MEMORY USAGE <key>
* CLIENT LIST 
* "pipelining"
  ```
  MULTI
  command1 
  command2 
  ...
  EXEC
  ```
* FLUSHALL [async] # remove all keys
  

## Key Spaces ( any binary represenation up to 512 Mb )
* Flat key space
* No automatic namespacing
* Logical Databases ( Database zero )
* Naming conventions ( case sensitive, example user:{unique id of user}:followers

## getting a list of existings keys
![keys list](https://i.postimg.cc/5ygnVyrt/redis-search-key-scan.png)
* keys
```redis
KEYS customer:15*
KEYS cus:15*
KEYS *
```
* scan
```
SCAN 0 MATCH customer*
SCAN 0
```
## retrieve values by key, save collection
```redis
TYPE <key>
OBJECT ENCODING <key>
```
* if value is of type string -> GET <key>
* if value is of type hash -> HGETALL <key>
* if value is of type lists -> 
  * LRANGE <key> <start> <end> ( hgetall )
* if value is of type sets -> 
  * SMEMBERS <key>
* if value is of type sorted sets -> 
  * ZRANGEBYSCORE <key> <min> <max>

## insert values, add value
* existence of keys
```
EXIST {key}
# EXIST customer:1500
```

* set value
```redis
# insert only if the record still Not eXists
SET customer:3000 Vitalii NX

# insert only if the record EXXists
SET customer:3000 cherkavi XX
```

## list (always ordered ) operations
  * LLEN
  * LRANGE <key> <start> <end> ( hgetall )
    ```LRANGE my-list 0 -1```
  * LINDEX ( get from specific position )
  * LPUSH ( left push )
  * RPUSH ( right push )
  * LSET 
  * LINSERT ( insert after certain element )
  * LPOP ( left pop )
  * RPOP ( right pop )
  * LREM ( remove element by value )
  * LTRIM ( remove to certain length, ``` LTRIM mylist -5 -1``` - retain only last 5 elements  )
    ```
    rpush mylist 1 2 3 4 5
    lstrim mylist 0 3
    lrange mylist 0 -1
    # 1 2 3
    ```

## set ( unordered )
  * SADD  
  * SMEMBERS <key>
  * SISMEMBER <key> <value> ( check if value present into set )
  * SCARD # amount of elements, CARDinality
  * SSCAN <key for searching> <number of cursor> MATCH <pattern>
    ```SSCAN myset 0 MATCH *o*```
  * SREM ( remove by value )
  * SPOP ( pop random!!! element  )
  * SUNION (sql:union)
  * SINTER (sql:inner join)
    ```
    sadd myset1 one two three four
    sadd myset2 three four five six
    sinter myset1 myset2
    ```
  * SDIFF ( not in )
    ```
    sadd set-three A b C 
    sadd set-four a b C
    sdiff set-three set-four # A
    ```

## set ( ordered )
  * ZRANGE <key> <rank/index start> <rank/index stop> # inclusive
  * ZRANGEBYSCORE <key> <score start> <score stop> # inclusive
  * ZRANGEBYLEX
    ```
    ZADD zset 10 aaa
    ZADD zset 20 bbb
    ZADD zset 30 ccc
    ZADD zset 30 ddd

    ZRANGEBYLEX zset "[aaa" "[ddd"
    ZRANGEBYLEX zset "[aaa" "(ddd"
    ```
  * ZADD <key>
  * ZREM <key> <value>
  * ZREMRANGEBYRANK
    ```
    redis:6379>zrange zset 0 -1
    1) "aaa"
    2) "bbb"
    3) "ccc"
    4) "ddd"
    5) "eee"
    redis:6379> ZREMRANGEBYRANK zset 4 5 # (not-inclusive inclusive]
    (integer) 1
    redis:6379> zrange zset 0 -1 # 
    1) "aaa"
    2) "bbb"
    3) "ccc"
    4) "ddd"
    ```
  * ZRANK <key> <value>
  * ZSCORE <key> <value>
  * ZCOUNT <key> <min score> <max score> # inclusive 
  * ZINTERSTORE <destination key> <number of keys> <key1, key2.... [number of keys]> WEIGHTS <for key1> <for key2> AGGREGATE <SUM|MIN|MAX> # intersection of sets (WEIGHT can be specified for all elements) and ordered-sets with multiplication factor WEIGHTS and way of AGGREGATion
  * ZUNIONSTORE <destination key> <number of keys> <key1, key2.... [number of keys]> WEIGHTS <for key1> <for key2> AGGREGATE <SUM|MIN|MAX> # union of sets (WEIGHT can be specified for all elements) and ordered-sets with multiplication factor WEIGHTS and way of AGGREGATion

## hash value (map, dictionary), set value, read hash value
> hash has only one level, can't be embeddable
```
HSET <key> <field1> <value1> <field2> <value2>
HGET <key> <field...>
HMGET <key> <field1> <field2>
HGETALL <key>
```

## increase value
```redis
INCR <key> # for integer
# SET my-personal-key 10
# SET my-personal-key "10"
# INCR my-personal-key
# INCRBY my-personal-key 3
# INCRBYFLOAT my-personal-key 2.5
```

## expiration for key
```
EXPIRE {key} {seconds}
PEXPIRE {key} {miliseconds}

EXPIREAT {key} {timestamp}
PEXPIREAT {key} {miliseconds-timestamp}
```
check expiration, check TimeToLive
```
TTL {key}
```
check living time
```
OBJECT IDLETIME <key>
```
remove expiration
```
PERSIST {key]
```
set with TTL
```
# miliseconds
SET customer:3000 warior PX 60000
# seconds
SET customer:3000 warior EX 60
```

## moving members, cut/paste members
```
SMOVE "source set" "destination set" "member name"
```

## delete 
```
# delete key and value with blocking until removing associated memory block
DEL {key}

# delete key without blocking
UNLINK {key}
```

![client architecture](https://i.postimg.cc/fTp83WSJ/redis-client.png)  
[clients libraries](https://redis.io/clients)  
![connection types](https://i.postimg.cc/rw7qqyR8/redis-deployment-connections.png)  
![redis-java types](https://i.postimg.cc/c4qj1KXk/redis-java-types.png)  

# Publish/Subscribe
```
SUBSCRIBE <channel name>
```
```
PUBLISH <channel name> <value>
```
* subscribers listening only for new messages
* published message will not be saved/stored ( if no subscribers are listening - lost forever ) - fire and forget
* message is just a string ( unstructured )
* no unique id for message 
![Pub/Sub vs Streams](https://i.postimg.cc/6QBRGhN6/redis-streams-vs-pubsub.png)

# Stream
* stream information
```
# XINFO GROUPS <name of stream >
XINFO GROUPS numbers
# XINFO STREAM <key == stream name>
XINFO STREAM numbers
# XINFO CONSUMERS <key == stream name> < name of consumer group >
XINFO CONSUMERS numbers numbers-group


# print all clients
CLIENT LIST
CLIENT SETNAME
```

![streams pub sub](https://i.postimg.cc/66rt4RwT/redis-streams-pub-sub.png)
![storage and delivery](https://i.postimg.cc/DzTSLhHK/redis-streams-storage-and-delivery.png)
* add stream entry https://redis.io/commands/xadd
```redis-cli
XADD <name of stream> <unique ID, or *> <field-name> <field-value>
# return generated ID ( in case of * ) like "<miliseconds>-<add digit>" or specified by user ID
# XADD my-stream * my-field 0
XADD numbers * n 6
XADD numbers * n 7
```

* calculate amount of messages 
```
# XLEN <stream name>
XLEN numbers
```
* print all messages 
```
# XRANGE <stream name> - +
XRANGE numbers - +
# XREVRANGE <stream name> + -
XREVRANGE numbers + -
```
* read specific messages 
```
# XREAD COUNT <count of values or -1 > STREAMS <stream-name stream-name2> <start id or 0>
# xread is waiting for last known client id, not inclusive; multistream
XREAD COUNT 1 STREAMS numbers 1570976182071-0
# XREAD BLOCK <milisec> STREAMS <stream-name> <last message id or '$' for last message> # waiting milisec (0-forever) for first new message
```

* Pending Entries List ![pending entries list](https://i.postimg.cc/jjsF475H/redis-consumer-pending.png)
> for adding consumer to ConsumerGroup - just read message
```
# XREADGROUP
# read all messages from Pending Entries List ( not acknowledged )
XREADGROUP GROUP numbers-group terminal-lower STREAMS numbers 0
# read new messages ( acknowledgement is not considering )
XREADGROUP GROUP numbers-group terminal-lower STREAMS numbers >
# read new messages ( switch off acknowledgement )
XREADGROUP GROUP numbers-group NOACK terminal-lower STREAMS numbers >
# read messages from stream <numbers> with group <numbers-group> with consumerA and after messageID ( non inclusive ) 1570997593499-0
XREADGROUP GROUP numbers-group consumerA STREAMS numbers 1570997593499-0
# read with waiting for new
XREADGROUP GROUP numbers-group terminal-lower COUNT 1 BLOCK 1000 STREAMS numbers 1570976183500
# read new messages with waiting for 1000 miliseconds ( acknowledgement is not considering )
XREADGROUP GROUP numbers-group terminal-lower COUNT 1 BLOCK 1000 STREAMS numbers >
```

* message acknowledges, removing from PendingEntriesList
```
# XACK <key of stream> <name of the group> <messageID>
XACK numbers numbers-group 1570976179060-0
```

```
# XDEL <stream name> ID ID...
# XTRIM <stream name> MAXLEN <length of latest messages in stream >
# more memory efficiency optimization
# XTRIM <stream name> MAXLEN ~ <length of latest messages in stream >
# trimming after adding value 
# XTRIM <stream name> MAXLEN ~ <length of latest messages in stream > <ID or *> <field-name> <field-value>
```

![assign consumer to partition](https://i.postimg.cc/2ykdGDdc/redis-partition-consumer.png)
![consumer groups](https://i.postimg.cc/dVn2BBqs/redis-consumer-groups.png)
![consumer in groups](https://i.postimg.cc/hjFYH5PB/redis-consumers-in-group.png)
```redis-cli
XGROUP CREATE <name of stream> <name of group> <message id>
XGROUP CREATE <name of stream> <name of group> <message id> MKSTREAM
# start from first message
# XGROUP CREATE my-stream my-group0 0
# start from next new message
# XGROUP CREATE my-stream my-group0 $
```


* data structure ( reading can be blocked and non-blocking  )
  ![new data structure](https://i.postimg.cc/qM6Hr3R1/redis-streams-new-data-structure.png)
  ![delete](https://i.postimg.cc/kgZcm22v/redis-stream-delete.png)
  ![trim](https://i.postimg.cc/RhVffm30/redis-streams-trim.png)
* acts like append-only list ( immutable, order cannot be changed)
  ![append only](https://i.postimg.cc/JhmfjYQF/redis-streams-append-only.png)
* each entries are hashes ( immutable )
  ![entry as a map](https://i.postimg.cc/Zn1gnwRV/redis-streams-entry-as-map.png)
* entries have unique ID - time entries
  ![default id](https://i.postimg.cc/K87VcryD/redis-streams-default-id.png) 
* supports ID-based range queries
  ![range queries](https://i.postimg.cc/sXLHHTps/redis-strams-range-query.png)
* fan-out
  ![](https://i.postimg.cc/1XK14P03/redis-consumer-fan-out.png)
* consumer groups
  ![consumer groups](https://i.postimg.cc/ZYG4CcXG/redis-streams-consumer-groups.png)
