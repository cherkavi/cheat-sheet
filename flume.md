# Architecture
contains from three tier:
* Agent tier has Flume agent installed
agent is sending data to Collect tier
* Collector tier 
aggregate the data push data to Storage tier
* Storage tier

Each tier has
* Source
* Sink

Source and Sink used Avro
( Remote procedure call and serialization framework )

Interceptors can be configured for simple data processing