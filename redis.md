
* [commands](https://redis.io/commands)
* [data types](https://redis.io/topics/data-types-intro)
* [REdis Serialization Protocol - RESP](https://redis.io/topics/protocol)  

## Key Spaces
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
```
* if value is of type string -> GET <key>
* if value is of type hash -> HGETALL <key>
* if value is of type lists -> 
  * LRANGE <key> <start> <end> ( hgetall )
  * LREM
  * RPOP
  * LLEN
  * RPUSH
* if value is of type sets -> 
  * SMEMBERS <key> ( HGET )
  * SCARD ( SSCAN )
  * SREM
  * SADD ( HSET )
  
* if value is of type sorted sets -> ZRANGEBYSCORE <key> <min> <max>

## insert values, add value
* existence of keys
```
EXIST {key}
# EXIST customer:1500
```

* set value
```
# insert only if the record still Not eXists
SET customer:3000 Vitalii NX

# insert only if the record EXXists
SET customer:3000 cherkavi XX
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

