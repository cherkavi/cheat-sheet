# gRPC

## links
* [gRPC Official Documentation](https://grpc.io/docs/)
* [gRPC GitHub Repository](https://github.com/grpc/grpc)
* [gRPC Tutorials and Examples](https://grpc.io/docs/quickstart/)

## 
```mermaid
flowchart LR

subgraph client [client side]
direction TB

ca[client application] -->|1| ced[client encoding]

ced -->|2 client| cr[gRPC Runtime]

cr -->|3| ctr[transport]
end

subgraph server [server side]
direction TB

ctr -->|4| str[transport]
str -->|5| sr[gRPC Runtime]
sr -->|6| sed[client decoding] 
sed -->|7| sa[server application]
end


sa --> sed
sed --> sr
sr --> str

str --> ctr
ctr --> cr
cr --> ced
ced --> ca

```