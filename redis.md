## Key Spaces
* Flat key space
* No automatic namespacing
* Logical Databases
* Naming conventions

[REdis Serialization Protocol - RESP](https://redis.io/topics/protocol)  
![client architecture](https://i.postimg.cc/fTp83WSJ/redis-client.png)  
[clients libraries](https://redis.io/clients)  
![connection types](https://i.postimg.cc/rw7qqyR8/redis-deployment-connections.png)  
![redis-java types](https://i.postimg.cc/c4qj1KXk/redis-java-types.png)  

## retrieve values by key, save collection
```redis
type <key>
```
* if value is of type string -> GET <key>
* if value is of type hash -> HGETALL <key>
* if value is of type lists -> 
  * lrange <key> <start> <end> ( hgetall )
  * lrem
  * rpop
  * llen
  * rpush
* if value is of type sets -> 
  * smembers <key> ( hget )
  * scard ( sscan )
  * srem
  * sadd ( hset )
  
* if value is of type sorted sets -> ZRANGEBYSCORE <key> <min> <max>

## moving members, cut/paste members
```
smove "source set" "destination set" "member name"
```
