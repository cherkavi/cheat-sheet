# Graph Database

## UI editor
* [G.V](https://gdotv.com/)

## JanusGraph

### start with docker

#### docker containers with link 
##### Janus server
```sh
docker rm janusgraph-default
docker run --name janusgraph-default janusgraph/janusgraph:latest
# --port 8182:8182
```
##### Gremlin console
```sh
docker run --rm --link janusgraph-default:janusgraph -e GREMLIN_REMOTE_HOSTS=janusgraph -it janusgraph/janusgraph:latest ./bin/gremlin.sh
```

#### docker container on localhost
##### Janus server
```sh
docker run --rm --name janusgraph-default --network="host" janusgraph/janusgraph:latest
```
##### Gremlin console
```sh
docker run --rm -e GREMLIN_REMOTE_HOSTS=localhost --network="host" -it janusgraph/janusgraph:latest ./bin/gremlin.sh
```

### connect with python
```sh
pip3 install gremlinpython
```
```py
from gremlin_python.process.anonymous_traversal import traversal
from gremlin_python.driver.driver_remote_connection import DriverRemoteConnection
g = traversal().withRemote(DriverRemoteConnection('ws://127.0.0.1:8182/gremlin','g'))

```