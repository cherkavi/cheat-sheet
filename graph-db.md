# Graph Database

## links
* [Gremlin reciepts](https://tinkerpop.apache.org/docs/3.5.5/recipes/)
* [Gremlin query language](https://docs.janusgraph.org/getting-started/gremlin/)  
* [SQL to Gremlin examples](http://sql2gremlin.com/)
* [Gremlin console](https://tinkerpop.apache.org/docs/3.5.5/tutorials/the-gremlin-console/)

## UI editor
* [G.V](https://gdotv.com/)
* [GraphML editor](https://www.yworks.com/yed-live/)

## [JanusGraph](https://docs.janusgraph.org/getting-started/basic-usage/)  
![janus gremlin](https://i.ibb.co/qd54MNh/2023-08-27-janus-gremlin.jpg)  

### start with docker

#### docker container on localhost ( avoid of installation )
```sh
# start Janus server
docker pull janusgraph/janusgraph:0.6
# x-www-browser https://hub.docker.com/r/janusgraph/janusgraph/tags
DOCKER_TAG=0.6
docker run --rm --name janusgraph-default --volume `pwd`:/workspace:rw --network="host" janusgraph/janusgraph:$DOCKER_TAG
# start Gremlin console
docker exec -e GREMLIN_REMOTE_HOSTS=localhost -it  janusgraph-default ./bin/gremlin.sh

# connect to Janus container
# docker exec -e GREMLIN_REMOTE_HOSTS=localhost -it  janusgraph-default /bin/bash
```

#### docker containers with link between containers
##### 
```sh
# Janus server
docker rm janusgraph-default
docker run --name janusgraph-default janusgraph/janusgraph:latest
# --port 8182:8182

# Gremlin console in separate docker container
docker run --rm --link janusgraph-default:janusgraph -e GREMLIN_REMOTE_HOSTS=janusgraph -it janusgraph/janusgraph:latest ./bin/gremlin.sh
```

### Gremlin console 
#### Embedded connection ( local )
```groovy
// with local connection
graph = TinkerGraph.open()
// g = traversal().withEmbedded(graph)
graph.features()

g = graph.traversal()
```

#### remote connection with submit
```groovy
// connect to database, during the start should be message in console like: "plugin activated: tinkerpop.server"
:remote connect tinkerpop.server conf/remote.yaml
// check connection
:remote
// --------- doesn't work:
// config = new PropertiesConfiguration()
// config.setProperty("clusterConfiguration.hosts", "127.0.0.1");
// config.setProperty("clusterConfiguration.port", 8182);
// config.setProperty("clusterConfiguration.serializer.className", "org.apache.tinkerpop.gremlin.driver.ser.GryoMessageSerializerV1d0");
// ioRegistries = org.janusgraph.graphdb.tinkerpop.JanusGraphIoRegistry
// config.setProperty("clusterConfiguration.serializer.config.ioRegistries", ioRegistries); // (e.g. [ org.janusgraph.graphdb.tinkerpop.JanusGraphIoRegistry) ]
// config.setProperty("gremlin.remote.remoteConnectionClass", "org.apache.tinkerpop.gremlin.driver.remote.DriverRemoteConnection");
// config.setProperty("gremlin.remote.driver.sourceName", "g");
// graph = TinkerGraph.open(config)
// --------- doesn't work:
// graph = EmptyGraph.instance().traversal().withRemote(config);
// g = graph.traversal()
```

#### remote connection without submit
```groovy
import static org.apache.tinkerpop.gremlin.process.traversal.AnonymousTraversalSource.traversal;
g = traversal().withRemote("conf/remote-graph.properties");
```

??? doesn't work with connecting points 
```groovy
// with remote connection
cluster = Cluster.build().addContactPoint('127.0.0.1').create()
client = cluster.connect()
// g = traversal().withRemote(DriverRemoteConnection.using("localhost", 8182));
g = new GraphTraversalSource(DriverRemoteConnection.using(client, 'g'))
```


#### gremlin simplest dataset example
> WARN: default connection is local, need to "submit" each time
```groovy
// insert data, add :submit if you did't connect to the cluster
// :submit g.addV('person').....
alice=g.addV('Alice').property('sex', 'female').property('age', 30).property('name','Alice');
bob=g.addV('Bob').property('sex', 'male').property('age', 35).property('name','Bob').as("Bob");
marie=g.addV('Marie').property('sex', 'female').property('age', 5).property('name','Marie');
g.tx().commit() // only for transactional TraversalSource

// select id of element
alice_id=g.V().hasLabel('Alice').id().next();
bob_id=g.V().has('name','Bob').id().next()
alice_id=g.V().has('name','Alice').id().next()
marie_id=g.V().has('name','Marie').id().next()
bob=g.V( bob_id )
alice=g.V( alice_id )
:show variables

// select vertex
g.V() \
 .has("sex", "female") \
 .has("age", lte(30)) \
 .valueMap("age", "sex", "name")
//  .values("age")       


// g.V().hasLabel('Bob').addE('wife').to(g.V().has('name', 'Alice'))
// The child traversal of [GraphStep(vertex,[]), HasStep([name.eq(Alice)])] was not spawned anonymously - use the __ class rather than a TraversalSource to construct the child traversal
g.V(bob_id).addE('wife').to(__.V(alice_id)) \
 .property("start_time", 2010).property("place", "Canada");

g.V().hasLabel('Bob').addE('daughter').to(__.V().has('name', 'Marie')) \
 .property("start_time", 2013).property("birth_place", "Toronto");

g.addE('mother').to(__.V(alice_id)).from(__.V(marie_id))


// select all vertices
g.V().id()

// select all edges
g.E()

// select data: edges out of
g.V().has("name","Bob").outE()
// select data: edges in to
g.V().has("name","Alice").inE().outV().values("name")
// select data: out edge(wife), point in 
g.V().has("name","Bob").outE("wife").inV().values("name")
// select data: out edge(wife), point to Vertext, in edge(mother), coming from 
g.V().has("name","Bob").outE("wife").inV().inE("mother").outV().values("name")


// remove Vertex
g.V('4280').drop()

// remove Edge
g.V().has("name", "Bob").outE("wife").drop()


:exit
```
#### [janus export db](https://tinkerpop.apache.org/docs/3.6.4/reference/#io-step)
```groovy
// export DB to GraphSON ( JSON )
g.io("/workspace/output.json").with(IO.writer, IO.graphson).write().iterate()
// import from GraphSON ( JSON ) to DB
g.io("/workspace/output.json").with(IO.reader, IO.graphson).read().iterate()

// export DB to GraphML ( XML )
g.io("/workspace/output.xml").with(IO.writer, IO.graphml).write().iterate()
// import from GraphML ( XML ) to DB
g.io("/workspace/output.xml").with(IO.reader, IO.graphml).read().iterate()

// ------------ doesn't work
// import db
// graph = JanusGraphFactory.open('conf/janusgraph.properties')
// graph = JanusGraphFactory.open('conf/remote-graph.properties')
// reader = g.graph.io(graphml()).reader().create()
// inputStream = new FileInputStream('/workspace/simple-dataset.graphml')
// reader.readGraph(inputStream, g.graph)
// inputStream.close()

g.V()
```



### [connect to janus with python  gremlin](https://tinkerpop.apache.org/docs/current/reference/#connecting-rgp)
```sh
# x-www-browser https://pypi.org/project/gremlinpython/
pip3 install gremlinpython
```
#### python obtain traversal
```py
from gremlin_python.process.anonymous_traversal import traversal
from gremlin_python.driver.driver_remote_connection import DriverRemoteConnection
g = traversal().withRemote(DriverRemoteConnection('ws://127.0.0.1:8182/gremlin','g'))
```
#### python obtain traversal
```py
from gremlin_python.process.graph_traversal import __
from gremlin_python.process.traversal import T
from gremlin_python.process.traversal import IO
from gremlin_python.structure.graph import Graph
from gremlin_python.driver.driver_remote_connection import DriverRemoteConnection

g = Graph().traversal().withRemote(DriverRemoteConnection('ws://localhost:8182/gremlin', 'g'))
```

#### python read values
```py
for each in g.V().valueMap(True).toList():
    print(each)

v1 = g.V().valueMap(True).toList()[0]
v1.id
v1.label
```

#### python drop values
```py
id_of_vertex=4096
g.V(id_of_vertex).drop().iterate()
```

#### script from file
```sh
# this path is **server path** not the local one
PATH_TO_EXPORT_JSON="/workspace/output.json"
g.io(PATH_TO_EXPORT_JSON).with_(IO.reader, IO.graphson).read().iterate()
```
