# NoSQL
## my own cheat-sheets
* [nosql.graphdb](./graph-db-cheat-sheet.md)
* [cassandra](./cassandra-cheat-sheet.md)
* [duckdb](./duckdb-cheat-sheet.md)
* [elastic](./elastic-cheat-sheet.md)
* [hbase](hbase-cheat-sheet.md)
* [influxdb](influxdb-cheat-sheet.md)
* [mongodb](mongodb-cheat-sheet.md)
* [neo4j](neo4j.md)
* [redis](redis-cheat-sheet.md)
* [solr](solr-cheat-sheet.md)

## databases by types
### scalable and ACID and (relational and/or sql -access)

#### [VoltDB](https://www.voltactivedata.com/)

#### [SenseiDB](https://dbsensei.com/)

#### [GenieDB](https://dbdb.io/db/geniedb)

#### [ScaleDB](https://www.usenix.org/conference/osdi23/presentation/mehdi)

#### [Drizzle](http://drizzle.org/)

#### [NDB MySQL Cluster](https://dev.mysql.com/downloads/cluster/)

#### [HandlerSocket (Percona)](https://docs.percona.com/legacy-documentation/percona-server-for-mysql/Percona-Server-for-MySQL-5.5.pdf)

#### ~~[JustOneDB](https://dune.com/tk-research/opbnb)~~

#### [Wakanda DB](https://wakanda.github.io/)

#### [MemSQL (new)](https://www.singlestore.com/blog/revolution/)

#### [NuoDB](http://www.nuodb.com/)

#### [SchemafreeDB](http://schemafreedb.com/)

#### [Cachelot](http://cachelot.io/)

### Wide Column Store / Column Families

#### [Hadoop / HBase](http://hadoop.apache.org/)
* API: **Java / any writer**, 
* Protocol: **any write call**, 
* Query Method: **MapReduce Java / any exec**, 
* Replication: **HDFS 
* Replication**, 
* Written in: **Java**, 
* Concurrency: ?, 
* Misc: **Links**: 3 Books \[[1](http://www.amazon.com/Hadoop-Action-Chuck-Lam/dp/1935182196/), [2](http://www.amazon.com/Pro-Hadoop-Jason-Venner/dp/1430219424/), [3](http://www.amazon.com/Hadoop-Definitive-Guide-Tom-White/dp/0596521979/)\], Article [\>>](https://www.guru99.com/hbase-architecture-data-flow-usecases.html)

#### [Cassandra](https://cassandra.apache.org/)
massively scalable, partitioned row store, masterless architecture, linear scale performance, no single points of failure, read/write support across multiple data centers & cloud availability zones. API / 
* Query Method: **CQL and Thrift**, 
* replication: **peer-to-peer**, 
* written in: **Java**, 
* Concurrency: **tunable consistency**, 
* Misc: built-in data compression, MapReduce support, primary/secondary indexes, security features. 
* Links: 
  * [Documentation](http://www.datastax.com/docs), 
  * [PlanetC\*](http://planetcassandra.org/), 
  * [Company](http://www.datastax.com/).

#### [Scylla](http://www.scylladb.com/)
Cassandra-compatible column store, with consistent low latency and more transactions per second. Designed with a thread-per-core model to maximize performance on modern multicore hardware. Predictable scaling. No garbage collection pauses, and faster compaction.

#### [Hypertable](http://hypertable.org/)
* API: **Thrift** (Java, PHP, Perl, Python, Ruby, etc.), 
* Protocol: **Thrift**, 
* Query Method: **HQL, native Thrift API**, 
* Replication: **HDFS Replication**, 
* Concurrency: **MVCC**, 
* Consistency Model: **Fully consistent** 
* Misc: High performance C++ implementation of Google's Bigtable. [» Commercial support](http://www.hypertable.com/)  

#### [Accumulo](http://accumulo.apache.org/)
Accumulo is based on **BigTable** and is built on top of [**Hadoop**](http://hadoop.apache.org/), [**Zookeeper**](http://zookeeper.apache.org/), and [**Thrift**](http://thrift.apache.org/). It features improvements on the BigTable design in the form of **cell-based access control**, improved **compression**, and a server-side programming mechanism that can modify key/value pairs at various points in the data management process.

#### [Amazon SimpleDB](http://aws.amazon.com/simpledb/)
* Misc: not open source / part of AWS, [Book](http://www.apress.com/book/view/1430225335) (will be outperformed by DynamoDB!)  

#### [Cloudata](http://www.cloudata.org/)
Google's Big table clone like HBase. [» Article](http://www.readwriteweb.com/hack/2011/02/open-source-bigtable-cloudata.php)

#### [MonetDB](https://www.monetdb.org/)
Column Store pioneer since 2002.

#### [HPCC](http://www.hpccsystems.com/)
from [LexisNexis](http://www.lexisnexis.com/), [article](http://wikibon.org/blog/lexisnexis-hpcc-takes-on-hadoop-as-battle-for-big-data-supremacy-heats-up/)

#### [Apache Flink](http://flink.apache.org/)
(formerly known as [Stratosphere](http://stratosphere.eu/)) 
massively parallel & flexible data analytics platform,   
* API: **Java, Scala**, 
* Query Method: **expressive data flows (extended M/R, rich UDFs, iteration support)**, 
* Data Store: **independent** (e.g., HDFS, S3, MongoDB), 
* Written in: Java, License: Apache License V2.0, 
* Misc: **good integration with Hadoop stack** (HDFS, YARN), source code on [Github](https://github.com/apache/flink)

#### [IBM Informix](http://www-01.ibm.com/software/data/informix/)
horizontally and vertically scalable, relational, partitioned row store, document store API / 
* Query Method: **SQL (native, DRDA, JDBC, ODBC), MongoDB wire listener, mixed mode**, 
* replication: **master / slave, peer-to-peer, sharding, grid operations**, 
* written in: **C**, 
* Concurrency: **row, page, table, db locking**, 
* Misc: ACID, built-in data compression, scheduler, automatic cyclic storage management, extensible, in memory acceleration, native ports from ARM v6 up 
* Links: [Documentation](http://www-01.ibm.com/support/knowledgecenter/SSGU8G_12.1.0/com.ibm.welcome.doc/welcome.htm), [IIUG](http://www.iiug.com/), [Company](http://www.ibm.com/).

#### [Splice Machine](http://www.splicemachine.com/)
Splice Machine is an RDBMS built on [Hadoop](http://hadoop.apache.org/), [HBase](http://hbase.apache.org/) and [Derby](http://db.apache.org/derby/). Scale real-time applications using commodity hardware without application rewrites, Features: **ACID transactions, ANSI SQL support, ODBC/JDBC, distributed computing**

#### ~~[eXtremeDB Financial Edition](http://financial.mcobject.com/)~~
massively scalable in-memory and persistent storage DBMS for analytics on market data (and other time series data). APIs: **C/C++, Java Native Interface (JNI), C#/.NET), Python, SQL (native, ODBC, JDBC)**, Data layout: **row, columnar, hybrid,** 
* Written in: **C**, 
* Replication: **master/slave, cluster, sharding,** 
* Concurrency: **Optimistic (MVCC) and pessimistic (locking)**

#### ~~[ConcourseDB](http://concoursedb.com/)~~
a distributed self-tuning database with 
* ~~[automatic indexing](http://concoursedb.com/blog/index-all-the-things/)~~
* version control and ACID transactions. 
* Written In: **Java**. API/
* Protocol: **Thrift (many languages)**. 
* Concurrency: **serializable transactions with 
* ~~[just-in-time locking](http://concoursedb.com/blog/just-in-time-locking/).~~
* Misc: uses a [buffered storage system](http://concoursedb.com/guide/storage-model/) to commit all data to disk immediately while perform rich indexing in the background.

#### [Druid](http://druid.io/)
open-source analytics 
* data store for business intelligence (OLAP) queries on event data. Low latency (real-time) data ingestion, flexible data exploration, fast data aggregation. Scaled to trillions of events & petabytes. Most commonly used to power user-facing analytic applications.   
* API: **JSON over HTTP**, APIs: **Python, R, Javascript, Node, Clojure, Ruby, Typescript + support SQL queries** 
* Written in: **Java** License: **Apache 2.0** 
* Replication: **Master/Slave**

#### ~~[KUDU](http://getkudu.io/)~~
Apache Kudu (incubating) completes Hadoop's storage layer to enable fast analytics on fast data.

#### [Elassandra](https://github.com/vroyer/elassandra)
Elassandra is a fork of Elasticsearch modified to run on top of Apache Cassandra in a scalable and resilient peer-to-peer architecture. Elasticsearch code is embedded in Cassanda nodes providing advanced search features on Cassandra tables and Cassandra serve as an Elasticsearch data and configuration store.
\[OpenNeptune, Qbase, KDI\]

### Document Store

#### [Elastic](http://www.elasticsearch.org/)
* API: **REST and many languages**, 
* Protocol: **REST**, 
* Query Method: **via JSON**, 
* Replication + Sharding: **automatic and configurable**, 
* written in: **Java**, 
* Misc: schema mapping, multi tenancy with arbitrary indexes, [» Company and Support](http://www.elasticsearch.com/), [» Article](https://www.found.no/foundation/elasticsearch-as-nosql/)

#### [ArangoDB](http://www.arangodb.com/)

#### [OrientDB](http://orientdb.com/)

#### [gunDB](http://gundb.io/)

#### [MongoDB](http://www.mongodb.org/)
* API: **BSON**, 
* Protocol: C, 
* Query Method: **dynamic object-based language & MapReduce**, 
* Replication: **Master Slave & Auto-Sharding**, 
* Written in: **C++**,
* Concurrency: **Update in Place**. 
* Misc: **Indexing, GridFS, Freeware + Commercial License** 
* Links: [» Talk](http://www.leadit.us/hands-on-tech/MongoDB-High-Performance-SQL-Free-Database), [» Notes](http://www.paperplanes.de/2010/2/25/notes_on_mongodb.html), [» Company](http://www.10gen.com/)

#### [Cloud Datastore](http://cloud.google.com/datastore)
A fully managed Document store with multi-master replication across data centers. Originally part of Google App Engine, it also has REST and gRPC APIs. Now [Firestore](https://firebase.googleblog.com/2017/10/introducing-cloud-firestore.html)?!

#### [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)
is a fully managed, globally distributed NoSQL database perfect for the massive scale and low latency needs of modern applications. Guarantees: 99.99% availability, 99% of reads at <10ms and 99% of writes at <15ms. Scale to handle 10s-100s of millions of requests/sec and replicate globally with the click of a button. APIs: .NET, .NET Core, Java, Node.js, Python, REST. Query: SQL. 
* Links: [» Service](https://azure.microsoft.com/services/documentdb/), [» Pricing](https://azure.microsoft.com/pricing/details/documentdb/), [» Playground](https://www.documentdb.com/sql/demo), [» Documentation](https://docs.microsoft.com/azure/documentdb/)

#### [RethinkDB](http://www.rethinkdb.com/)
* API: **protobuf-based**, 
* Query Method: **unified chainable query language (incl. JOINs, sub-queries, MapReduce, GroupedMapReduce)**; 
* Replication: **Sync and Async Master Slave with per-table acknowledgements**, Sharding: **guided range-based**, 
* Written in: **C++**, 
* Concurrency: **MVCC**. 
* Misc: log-structured storage engine with concurrent incremental garbage compactor

#### [Couchbase Server](http://www.couchbase.com/)
* API: Memcached API+protocol** (binary and ASCII) , **most languages**, 
* Protocol: **Memcached REST interface for cluster conf + management**, 
* Written in: **C/C++** + **Erlang** (clustering), 
* Replication: **Peer to Peer, fully consistent**, 
* Misc: **Transparent topology changes during operation, provides memcached-compatible caching buckets, commercially supported version available**, 
* Links: [» Wiki](http://wiki.membase.org/), [» Article](http://www.infoq.com/news/2010/10/membase)

#### [CouchDB](http://couchdb.apache.org/)
* API: **JSON**, 
* Protocol: **REST**, 
* Query Method: **MapReduceR of JavaScript Funcs**, 
* Replication: **Master Master**, 
* Written in: **Erlang**, 
* Concurrency: **MVCC**
* **Links**: 
  * [» 3 CouchDB books](http://couchdb.apache.org/docs/books.html)
  * ~~[Couch Lounge](http://tilgovi.github.com/couchdb-lounge/)~~ 
  * [» Dr. Dobbs](http://www.drdobbs.com/java/223100116)

#### ~~[ToroDB](http://www.torodb.com/)~~
* API: **MongoDB API** and **SQL**, 
* Protocol: MongoDB Wire Protocol / MongoDB compatible, 
* Query Method: **dynamic object-based language & SQL**, 
* Replication: **RDBMS Backends' 
* Replication System & Support for 
* replication from MongoDB's Replica Set**, 
* Written in: **Java**, 
* Concurrency: **MVCC**. 
* Misc: **Open Source NoSQL and SQL database**. The agileness of a doc DB with the reliability and the native SQL capabilities of PostgreSQL. 

#### [SequoiaDB](http://www.sequoiadb.com/en/index.php?p=index&j=2)
* API: **BSON**, 
* Protocol: **C**, 
* Query Method: **dynamic object-based language**, 
* Replication: **Master Slave & Auto-Sharding**, 
* Written in: **C++**, 
* Misc: Indexing, Large Object Store, Transaction, Free + Commercial License, [Benchmark](http://www.bankmark.de/wp-content/uploads/2014/12/bankmark-20141201-WP-NoSQLBenchmark.pdf), [Code](https://github.com/SequoiaDB/SequoiaDB)

#### [NosDB](http://www.alachisoft.com/nosdb/)
a 100% native **.NET** Open Source **NoSQL Document Database** (Apache 2.0 License).   
It also supports **SQL** querying over JSON Documents.   
Data can also be accessed through **LINQ & ADO.NET**.   
NosDB also provides strong **server-side and client-side caching** features by integrating NCache.

#### [RavenDB](http://github.com/ravendb/ravendb)
.Net solution. Provides **HTTP/JSON** access. **LINQ** queries & **Sharding** supported.  
[Misc](http://www.codeproject.com/KB/cs/RavenDBIntro.aspx)

#### [MarkLogic Server](http://www.marklogic.com/)
(developer+commercial)   
* API: **JSON, XML, Java** 
* Protocols: **HTTP, REST** 
* Query Method: **Full Text Search and Structured Query, XPath, XQuery, Range, Geospatial, Bitemporal** 
* Written in: **C++** 
* Concurrency: **Shared-nothing cluster, MVCC** 
* Misc: Petabyte-scalable and elastic (on premise in the cloud), ACID + XA transactions, auto-sharding, failover, master slave 
* replication (clusters), 
* replication (within cluster), high availablity, disaster recovery, full and incremental backups, government grade security at the doc level, developer community [»](http://developer.marklogic.com/)

#### [Clusterpoint Server](http://www.clusterpoint.com/)
(freeware+commercial)   
* API: **XML, PHP, Java, .NET** 
* Protocols: **HTTP, REST, native TCP/IP** 
* Query Method: **full text search, XML, range and Xpath queries**; 
* Written in **C++** 
* Concurrency: **ACID-compliant, transactional, multi-master cluster** 
* Misc: Petabyte-scalable document store and full text search engine. Information ranking. 
* Replication. Cloudable.

#### [JSON ODM](https://github.com/konsultaner/jsonOdm)
Object Document Mapper for JSON-Documents 
* written in pure **JavaScript**. It queries the collections with a **gremlin**\-like DSL that uses **MongoDB's API** methods, but also provides joining. The collections extend the native array objects, which gives the overall ODM a good performance. Queries 500.000 elements in less then a second.

#### [NeDB](https://github.com/louischatriot/nedb)
NoSQL database for Node.js in pure **javascript**. It implements the most commonly used subset of **MongoDB's API** and is quite **fast** (about 25,000 reads/s on a 10,000 documents collection with indexing).

#### [Terrastore](http://code.google.com/p/terrastore/)
  
* API: **Java & http**, 
* Protocol: **http**, Language: **Java**, Querying: **Range queries, Predicates**, 
* Replication: **Partitioned with consistent hashing**, Consistency: **Per-record strict consistency**, 
* Misc: Based on Terracotta

#### ~~[AmisaDB](http://www.amisalabs.com/)~~
Architected to unify the best of search engine, NoSQL and NewSQL DB technologies.   
* API: REST and many languages. 
* Query method: **SQL**. 
* Written in **C++**. 
* Concurrency: **MVCC**. 
* Misc: **ACID** transactions, data distribution via **consistent hashing**, **static and dynamic schema** support, **in-memory** processing. Freeware + Commercial License

#### [JasDB](http://www.oberasoftware.com/)
Lightweight open source document database 
* written in Java for high performance, runs in-memory, supports Android.   
* API: **JSON, Java** 
* Query Method: **REST OData Style Query language, Java fluent Query API** 
* Concurrency: **Atomic document writes** Indexes: **eventually consistent indexes**

#### [RaptorDB](http://www.codeproject.com/Articles/375413/RaptorDB-the-Document-Store)
JSON based, Document store database with compiled **.net map functions** and automatic hybrid bitmap indexing and **LINQ query filters**

#### ~~[djondb](http://djondb.com/)~~
* API: **BSON**, 
* Protocol: **C++**, 
* Query Method: **dynamic queries and map/reduce**, Drivers: **Java, C++, PHP** 
* Misc: ACID compliant, Full shell console over google v8 engine, djondb requirements are submited by users, not market. License: GPL and commercial

#### [EJDB](http://ejdb.org/)
Embedded JSON database engine based on tokyocabinet.   
* API: **C/C++, C# (.Net, Mono), Lua, Ruby, Python, Node.js binding**, 
* Protocol: **Native**, 
* Written in: **C**, Query language: **mongodb-like dynamic queries**, 
* Concurrency: **RW locking, transactional** , 
* Misc: **Indexing, collection level rw locking, collection level transactions, collection joins.**, License: **LGPL**

#### [densodb](http://www.densodb.net/)
DensoDB is a new NoSQL document database. 
* Written for .Net environment in c# language. It’s simple, fast and reliable. [Source](https://github.com/teamdev/densodb)

#### [SisoDB](http://www.sisodb.com/)
A Document Store on top of SQL-Server.

#### [SDB](http://pagenotes.com/wordpress/2011/12/08/sdb/)
For small online databases, PHP / JSON interface, implemented in PHP.

#### [NoSQL embedded db](https://github.com/petersirka/nosql)
Node.js asynchronous NoSQL embedded database for small websites or projects. Database supports: insert, update, remove, drop and supports views (create, drop, read). 
* Written in JavaScript, no dependencies, implements small 
* concurrency model.

#### [ThruDB](http://code.google.com/p/thrudb/)
Uses Apache [Thrift](http://incubator.apache.org/thrift/) to integrate multiple backend databases as BerkeleyDB, Disk, MySQL, S3.

#### [iBoxDB](http://www.iboxdb.com/)
Transactional embedded database, it can embed into mobile, desktop and web applications, supports on-disk and in-memory storages.   
* API: **Java,C# (Android, Mono, Xamarin, Unity3D)**. 
* Query Method: **SQL-like** and **KeyValue**. 
* Written In: **Java, C#**. 
* Replication: **MasterSlave, MasterMaster**.

#### [BergDB](http://www.bergdb.com/)
* API: **Java/.NET**. 
* Written in: **Java**. 
* Replication: **Master/Slave**. License: **AGLP**. Historical queries. ACID. Schemaless. 
* Concurrency: **STM and persistent data structure**. Append-only storage. Encrypted storage. Flexible durability control. Secondary & composite indexes. Transparently serializes Java/.NET objects.

#### [ReasonDB](https://github.com/anywhichway/reasondb)
100% JavaScript automatically synchronizing multi-model database with a SQL like syntax (JOQULAR) and swapable persistence stores. It supports joins, nested matches, projections or live object result sets, asynchronous cursors, streaming analytics, 18 built-in predicates, in-line predicates, predicate extensibility, indexable computed values, fully indexed Dates and Arrays, built in statistical sampling. Persistence engines include files, Redis, LocalStorage, block storage, and more.

#### [IBM Cloudant](https://cloudant.com/)
Cloud based, open-source, zero-config. Based on CouchDB and BigCouch.

#### ~~[BagriDB](http://bagridb.com/)~~
* API: **XQJ/XDM, REST, OpenAPI,** 
* Protocols: **Java, HTTP**, 
* Query Method: **distributed XQuery + server-side XQuery/Java extensions**, 
* Written in: **Java**, 
* Concurrency: **MVCC**, Document format: **XML, JSON, POJO, Avro, Protobuf**, etc. 
* Misc: in-memory Data and Computation Grid, transparent 
* replication and fail-over, true horizontal scalability, ACID transactions, rich indexeng and trigger capabilities, plugable persistent store, plugable data formats. [GitHub](https://github.com/dsukhoroslov/bagri)

### Key Value / Tuple Store

#### [DynamoDB](http://aws.amazon.com/dynamodb/)
Automatic ultra scalable NoSQL DB based on fast SSDs. Multiple Availability Zones. Elastic MapReduce Integration. Backup to S3 and much more...

#### [Azure Table Storage](http://msdn.microsoft.com/en-us/library/dd179423.aspx)
Collections of free form entities (row key, partition key, timestamp). Blob and Queue Storage available, 3 times redundant. Accessible via REST or ATOM.

#### [Riak](http://basho.com/products/#riak)  
* API: **tons of languaes, JSON**, 
* Protocol: **REST**, 
* Query Method: **MapReduce term matching** , Scaling: **Multiple Masters**; 
* Written in: **Erlang**, 
* Concurrency: **eventually consistent** (stronger then MVCC via Vector Clocks)

#### [Redis](http://redis.io/)
* API: **Tons of languages**, 
* Written in: **C**, 
* Concurrency: **in memory** and saves asynchronous disk after a defined time. Append only mode available. Different kinds of fsync policies. 
* Replication: **Master / Slave**, 
* Misc: **also lists, sets, sorted sets, hashes, queues**. Cheat-Sheet: [»](http://masonoise.files.wordpress.com/2010/03/redis-cheatsheet-v1.pdf), great slides [»](http://blog.simonwillison.net/post/57956858672/redis) Admin UI [»](http://www.servicestack.net/mythz_blog/?p=381) From the Ground up [»](http://blog.mjrusso.com/2010/10/17/redis-from-the-ground-up.html)

#### [Aerospike](http://www.aerospike.com/)
Fast and web-scale database. RAM or SSD. Predictable performance; achieves 2.5 M TPS (reads and writes), 99% under 1 ms. Tunable consistency. Replicated, zero configuration, zero downtime, auto-clustering, rolling upgrades, Cross Datacenter 
* Replication (XDR). 
* Written in: C. APIs: C, C#, Erlang, Go, Java, Libevent, Node, Perl, PHP, Python, Ruby. 
* Links: [Community](http://www.aerospike.com/community), [Development](http://www.aerospike.com/develop), [Slides](http://www.slideshare.net/AerospikeDB/flash-economics-and-lessons-learned-from-operating-low-latency-platforms-at-high-throughput-with-aerospike-nosql), [2.5M TPS on 1 node at Intel](http://www.intel.com/content/dam/www/public/us/en/documents/solution-briefs/xeon-e5-v3-ssd-aerospike-nosql-brief.pdf), [1M TPS on AmazonWS](http://highscalability.com/blog/2014/8/18/1-aerospike-server-x-1-amazon-ec2-instance-1-million-tps-for.html), [1M TPS w/ SSDs on 1 Server](https://communities.intel.com/community/itpeernetwork/blog/2015/02/17/reaching-one-million-database-transactions-per-second-aerospike-intel-ssd), [Combating Memory Fragmentation](http://highscalability.com/blog/2015/3/17/in-memory-computing-at-aerospike-scale-when-to-choose-and-ho.html)

#### [LevelDB](http://code.google.com/p/leveldb/)
**Fast** & Batch updates. DB from **Google**. 
* [Written in C++. Blog](http://google-opensource.blogspot.com/2011/07/leveldb-fast-persistent-key-value-store.html), 
* [hot Benchmark](http://leveldb.googlecode.com/svn/trunk/doc/benchmark.html)
* [Article](http://www.golem.de/1107/85298.html)
* Java [access](https://github.com/fusesource/leveldbjni). 
* [C# (for Universal Windows Platform)](https://github.com/maxpert/LevelDBWinRT).

#### [RocksDB](http://rocksdb.org/)
* API: **C++**. 
* Written in C++. Facebook\`s improvements to Google\`s LevelDB to speed throughput for datasets larger than RAM. Embedded solution.

#### [Berkeley DB](http://www.oracle.com/technetwork/database/database-technologies/berkeleydb/overview/index.html)
* API: **Many languages**, 
* Written in: **C**, 
* Replication: **Master / Slave**, 
* Concurrency: **MVCC**, License: **Sleepycat**, [Berkeley DB Java Edition](http://www.oracle.com/technetwork/database/berkeleydb/overview/index-093405.html):   
* API: **Java**, 
* Written in: **Java**, 
* Replication: **Master / Slave**, 
* Concurrency: **serializable transaction isolation**, License: **Sleepycat**

#### [GenieDB](http://www.geniedb.com/)
Immediate consistency sharded KV store with an eventually consistent AP store bringing eventual consistency issues down to the theoretical minimum. It features efficient record coalescing. GenieDB speaks SQL and co-exists / do intertable joins with SQL RDBMs.

#### [BangDB](http://www.iqlect.com/)
* API: **Get,Put,Delete**, 
* Protocol: **Native, HTTP,** Flavor: **Embedded, Network, Elastic Cache**, 
* Replication: **P2P based Network Overlay**, 
* Written in: **C++**, 
* Concurrency: ?, 
* Misc: robust, crash proof, Elastic, throw machines to scale linearly, Btree/Ehash

#### [Chordless](http://sourceforge.net/projects/chordless/)
* API: **Java & simple RPC to vals**, 
* Protocol: **internal**, 
* Query Method: **M/R inside value objects**, Scaling: **every node is master for its slice of namespace**, 
* Written in: **Java**, 
* Concurrency: **serializable transaction isolation**,

#### [Scalaris](http://code.google.com/p/scalaris/)
* Written in: **Erlang**, 
* Replication: **Strong consistency over replicas**, 
* Concurrency: **non blocking Paxos**.

#### ~~[Tokyo Cabinet / Tyrant](http://fallabs.com/tokyocabinet/)~~
* **Links**: 
  * nice talk [»](http://www.infoq.com/presentations/grigorik-tokyo-cabinet-recipes), 
  * slides [»](http://www.scribd.com/doc/12016121/Tokyo-Cabinet-and-Tokyo-Tyrant-Presentation), 

#### [Scalien](http://scalien.com/)
* Protocol: **http** (text, html, JSON)**, C, C++, Python**, Java, Ruby, PHP,Perl. 
* Concurrency: **Paxos**.

#### ~~[Voldemort](http://project-voldemort.com/)~~
Open-Source implementation of Amazons Dynamo Key-Value Store.

#### [Dynomite](http://wiki.github.com/cliffmoon/dynomite/dynomite-framework)
Open-Source implementation of Amazons Dynamo Key-Value Store. 
* written in Erlang. With "data partitioning, versioning, and read repair, and user-provided storage engines provide persistence and query processing".

#### [KAI](http://sourceforge.net/projects/kai/)
Open Source Amazon Dnamo implementation, 
* Misc: [slides](http://www.slideshare.net/takemaru/kai-an-open-source-implementation-of-amazons-dynamo-472179)

#### ~~[MemcacheDB](http://memcachedb.org/)~~
* API: **Memcache protocol** (get, set, add, replace, etc.), 
* Written in: **C**, Data Model: **Blob**, 
* Misc: Is Memcached writing to BerkleyDB.

#### [Faircom C-Tree](http://www.faircom.com/nosql)
* API: **C, C++, C#, Java, PHP, Perl**, 
* Written in: **C,C++**. 
* Misc: **Transaction logging. Client/server. Embedded. SQL wrapper** (not core). Been around since 1979.  

#### [LSM](http://www.sqlite.org/src4/artifact/41b08c1d31c156d3916558aad89b7e7ae8a381c5)
Key-Value database that was written as part of SQLite4,  
They claim it is faster then LevelDB.  
Instead of supporting custom comparators, they have a recommended data encoding for keys that allows various data types to be sorted.

#### [KitaroDB](http://www.kitarodb.com/)
A fast, efficient on-disk 
* data store for **Windows Phone 8, Windows RT, Win32 (x86 & x64)** and **.NET**. Provides for key-value and multiple segmented key access. APIs for **C#, VB, C++, C** and **HTML5/JavaScript**. 
* Written in **pure C** for high performance and low footprint. Supports async and synchronous operations with 2GB max record size.

#### [upscaledb](http://upscaledb.com/)
embedded solution
* API: **C, C++, .NET, Java, Erlang.** 
* Written in **C,C++**. Fast key/value store with a parameterized B+-tree. Keys are "typed" (i.e. 32bit integers, floats, variable length or fixed length binary data). Has built-in analytical functions like SUM, AVERAGE etc.

#### [STSdb](http://stsdb.com/)
* API: **C#**, 
* Written in **C#**, embedded solution, generic XTable<TKey,TRecord> implementation, ACID transactions, snapshots, table versions, shared records, vertical data compression, custom compression, composite & custom primary keys, available backend file system layer, works over multiple volumes, petabyte scalability, LINQ.

#### [Tarantool/Box](https://github.com/mailru/tarantool)
* API: **C, Perl, PHP, Python, Java and Ruby**. 
* Written in: **Objective C** ,
* Protocol: **asynchronous binary, memcached, text (Lua console)**. Data model: **collections of dimensionless tuples, indexed using primary + secondary keys**. 
* Concurrency: **lock-free in memory, consistent with disk (write ahead log).** 
* Replication: **master/slave, configurable**. Other: **call Lua stored procedures.**

#### [Chronicle Map](https://github.com/OpenHFT/Chronicle-Map)
In-memory (opt. persistence via mmap), highly concurrent, low-latency key-value store.   
* API: **Java**. 
* Written in: **Java**. 
* Protocol: in-process Java, remote via [Chronicle Engine](http://chronicle.software/products/chronicle-engine/) + Wire: binary, text, Java, C# bindnings. 
* Concurrency: in-memory lock striping, read-write locks. 
* Replication: **multi-master, eventually consistent**.

#### [Maxtable](http://code.google.com/p/maxtable/)
* API: **C**, 
* Query Method: **MQL, native API**, 
* Replication: **DFS 
* Replication**, Consistency: **strict consistency** 
* Written in: **C**.

#### [Pincaster](http://github.com/jedisct1/Pincaster)
For geolocalized apps. 
* Concurrency: **in-memory with asynchronous disk writes**.   
* API: **HTTP/JSON**. 
* Written in: **C**. License: **BSD**.

#### [RaptorDB](http://www.codeproject.com/KB/database/RaptorDB.aspx)
A pure key value store with optimized b+tree and murmur hashing. (In the near future it will be a JSON document database much like mongodb and couchdb.)

#### [TIBCO Active Spaces](https://ssl.tibcommunity.com/blogs/activespaces)
peer-to-peer distributed in-memory (with persistence) datagrid that implements and expands on the concept of the Tuple Space. Has SQL Queries and ACID (=> NewSQL).

#### [allegro-C](http://www.allegro-c.de/)
Key-Value concept. Variable number of keys per record. Multiple key values, Hierarchic records. Relationships. Diff. record types in same DB. Indexing: B\*-Tree. All aspects configurable. Full scripting language. Multi-user ACID. Web interfaces (PHP, Perl, ActionScript) plus Windows client.

#### [nessDB](https://github.com/shuttler/nessDB)
A fast key-value Database (using LSM-Tree storage engine),   
* API: **Redis protocol** (SET,MSET,GET,MGET,DEL etc.), 
* Written in: **ANSI C**

#### ~~[HyperDex](http://hyperdex.org/)~~
Distributed searchable key-value store. Fast (latency & throughput), scalable, consistent, fault tolerance, using hyperscpace hashing. APIs for C, C++ and Python.

#### [SharedHashFile](https://github.com/simonhf/sharedhashfile)
Fast, open source, shared memory (using memory mapped files e.g. in /dev/shm or on SSD), multi process, hash table, e.g. on an 8 core i7-3720QM CPU @ 2.60GHz using /dev/shm, 8 processes combined have a 12.2 million / 2.5 to 5.9 million TPS read/write using small binary keys to a hash filecontaining 50 million keys. Uses sharding internally to mitigate lock contention. 
* Written in **C**.

#### [Symas LMDB](http://symas.com/mdb/)
Ultra-fast, ultra-compact key-value embedded 
* data store developed by Symas for the OpenLDAP Project. It uses memory-mapped files, so it has the read performance of a pure in-memory database while still offering the persistence of standard disk-based databases, and is only limited to the size of the virtual address space, (it is not limited to the size of physical RAM)

#### ~~[Sophia](http://sphia.org/)~~
Sophia is a modern embeddable key-value database designed for a high load environment. It has unique architecture that was created as a result of research and rethinking of primary algorithmical constraints, associated with a getting popular Log-file based data structures, such as LSM-tree. Implemented as a small C-
* written, BSD-licensed library.

#### [NCache](http://www.alachisoft.com/ncache/)
.NET Open Source Distributed Cache. 
* Written in C#. API .NET & Java. Query Parallel SQL Query, LINQ & Tags. 
* Misc: Linear Scalability, High Availability, WAN 
* Replication, GUI based Administration & Monitoring Tools, Messaging, Runtime Data Sharing, Cache & Item Level Events, Continuous Query & Custom Events, DB Dependencies & Expirations

#### [TayzGrid](http://www.alachisoft.com/tayzgrid/)
Open Source In-Memory JCache compliant Data Grid. 
* Written in Java. API Java, JCache JSR 107 & .NET. Query SQL & DB Synchronization. 
* Misc: Linear Scalability, High Availability, WAN 
* Replication, GUI based Administration & Monitoring Tools, Distributed Messaging, MapReduce, Entry Processor and Aggregator

#### [PickleDB](http://pythonhosted.org/pickleDB/)
Redis inspired K/V store for Python object serialization.

#### [Mnesia](http://www.erlang.org/doc/apps/mnesia/index.html)
(ErlangDB [»](http://www.infoq.com/news/2007/08/mnesia))

#### ~~[LightCloud](http://opensource.plurk.com/LightCloud/)~~
(based on Tokyo Tyrant)

#### [Hibari](http://hibari.sourceforge.net/)
Hibari is a highly available, strongly consistent, durable, distributed key-value 
* data store

#### [OpenLDAP](http://highlandsun.com/hyc/mdb/)
Key-value store, B+tree. Lightning fast reads+fast bulk loads. Memory-mapped files for persistent storage with all the speed of an in-memory database. No tuning conf required. Full ACID support. MVCC, readers run lockless. Tiny code, 
* written in C, compiles to under 32KB of x86-64 object code. Modeled after the BerkeleyDB API for easy migration from Berkeley-based code. Benchmarks against LevelDB, Kyoto Cabinet, SQLite3, and BerkeleyDB are available, plus full paper and presentation slides.

#### [Genomu](http://genomu.com/)
High availability, 
* concurrency-oriented event-based K/V database with transactions and causal consistency. 
* Protocol: **MsgPack**,   
* API: **Erlang, Elixir, Node.js**. 
* Written in: [Elixir](http://elixir-lang.org/), [Github-Repo](https://github.com/genomu/genomu).

#### [BinaryRage](https://github.com/mchidk/BinaryRage)
BinaryRage is designed to be a lightweight ultra fast key/value store for .NET with no dependencies. Tested with more than 200,000 complex objects 
* written to disk per second on a crappy laptop :-) No configuration, no strange driver/connector, no server, no setup - simply reference the dll and start using it in less than a minute.

#### [Elliptics](http://www.ioremap.net/projects/elliptics/)
Github Page [»](https://github.com/reverbrain/elliptics)

#### [DBreeze](https://github.com/hhblaze/DBreeze)
Professional, open-source, NoSql (embedded Key/Value storage), transactional, ACID-compliant, multi-threaded, object database management system for .NET 3.0> MONO. 
* Written in C#.

#### [TreodeDB](https://github.com/Treode/store)
* API: **Scala**. 
* Written in Scala. 
* Replication: **Replicas vote on writes and reads**. Sharding: **Hashes keys onto array of replica cohorts.** 
* Concurrency: **Optimistic + Multiversion 
* Concurrency Control. Provides multirow atomic writes. Exposes optimistic 
* concurrency through API to support HTTP Etags.** Embedded solution.

#### [BoltDB](https://github.com/boltdb/bolt)
Key/Value DB 
* written in Go.

#### ~~[Serenety](http://serenitydb.org/)~~
Serenity database implements basic Redis commands and extends them with support of Consistent Cursors, ACID transactions, Stored procedures, etc. The database is designed to store data bigger then available RAM.

#### [Cachelot](http://cachelot.io/)
* API: **Memcached**. 
* Written in **C++**. In-memory LRU cache with very small memory footprint. Works within fixed amount of memory. Cachelot has a C++ cache library and stand-alone server on top of it.

#### [filejson](https://www.npmjs.com/package/filejson)
Use a JSON encoded file to automatically save a JavaScript value to disk whenever that value changes. A value can be a Javascript: string, number, boolean, null, object, or an array. The value can be structured in an array or an object to allow for more complex 
* data stores. These structures can also be nested. As a result, you can use this module as a simple document store for storing semistructured data.

#### [InfinityDB](https://boilerbay.com/infinitydb/)
InfinityDB is an all-Java embedded DBMS with access like java.util.concurrent.ConcurrentNavigableMap over a tuple space, enhanced for nested Maps, LOBs, huge sparse arrays, wide tables with no size constraints. Transactions, compression, multi-core 
* concurrency, easy schema evolution. Avoid the text/binary trap: strongly-typed, fine-grained access to big structures. 1M ops/sec. Commercial, closed source, patented.

#### [KeyVast](https://github.com/keyvast/keyvast)
Key Value Store with scripting language. Data types include list, dictionary and set. Hierarchical keys.   
* API: **TCP/IP**, 
* Protocol: **Query language**, 
* Written in: **Delphi**, License: **MIT**, 
* Links: [Github](https://github.com/keyvast/keyvast)

#### [SCR Siemens Common Repository](http://www.convergence-creators.siemens.com/common-repository-telco-data-storage.html)
In-Memory, scale-by-cell division (under load), multi-tiered scalability (transactions, entries, indicies), read=write, geo-redundancy, redundancy per datatype, LDAP, API, bulkload facility for VNF state resiliency, VNF-M integratable, self-\[re-\]balancing, Backend for Mobile Network Functions

#### [IOWOW](http://iowow.io/)
The C/C++ persistent key/value storage engine based on **[skip list](https://en.wikipedia.org/wiki/Skip_list)** data structure.   
* API: **C/C++**. 
* Protocol: **Native**. Writen in: **C11**, 
* Concurrency: **RW locking**. License: **MIT**

#### [BBoxDB](https://github.com/jnidzwetzki/bboxdb/)
A distributed 
* data store for multi-dimensional data. BBoxDB enhances the key-value data model by a bounding box, which describes the location of a value in an n-dimensional space. Data can be efficiently retrieved using hyperrectangle queries. Spatial joins and dynamic data redistribution are also supported.   
* API: **Java**, 
* Protocol: **asynchronous binary**, Data model: **Key-bounding-box-value**, Scaling: **Auto-Sharding, 
* Replication**, 
* Written in: **Java**, 
* Concurrency: **eventually consistent / RW locking**

#### [NuSTER](https://github.com/jiangwenyuan/nuster)
A HTTP based, user facing, RESTful NoSQL cache server based on HAProxy. It can be used as an internal NoSQL cache sits between your application and database like Memcached or Redis as well as a user facing NoSQL cache that sits between end user and your application. It supports headers, cookies, so you can store per-user data to same endpoint. 
* Protocol: **HTTP**. Writen in: **C**.

#### [JDX](http://js2dx.com/)
A lightweight in-memory document-oriented database 
* written with JavaScript. 
* Includes Single Page Application API, 
* node serialization, 
* tree browsing and CRUD operations on document tuples through web GUI. 
* Integrate with server-side noSQL database instance into a transaction-synchronous cluster, 
* create SPA content or browse and serialize document tuples with HTML interface.

#### [Scality](http://www.scality.com/)
#### ~~[KaTree](http://www.katree.net/)~~
#### [TomP2P](http://tomp2p.net/)
#### [Kumofs](https://github.com/etolabo/kumofs)
#### [TreapDB](http://code.google.com/p/treapdb/)
#### [Wallet](https://github.com/YaroslavGaponov/wallet)
#### [NoSQLz](http://nosqlz.com/)
#### ? SubRecord
#### ? Mo8onDb
#### ? Dovetaildb

### Graph Databases

#### [Neo4J](http://www.neo4j.org/)
* API: **lots of langs**, 
* Protocol: **Java embedded / REST**, 
* Query Method: **SparQL, nativeJavaAPI, JRuby**, 
* Replication: **typical MySQL style master/slave**, 
* Written in: **Java**, 
* Concurrency: **non-block reads, writes locks involved nodes/relationships until commit**, 
* Misc: **ACID** possible, 
* Links: Video [»](http://www.infoq.com/presentations/emil-eifrem-neo4j), Blog [»](http://blog.neo4j.org/)

#### [ArangoDB](http://www.arangodb.com/), 
#### [OrientDB](http://orientdb.com/)
#### [gunDB](http://gundb.io/) 

#### [Infinite Graph](http://www.infinitegraph.com/)
* API: **Java**, 
* Protocol: **Direct Language Binding**, 
* Query Method: **Graph Navigation API,** **Predicate Language Qualification**, 
* Written in: **Java (Core C++)**, Data Model: **Labeled Directed Multi Graph**, 
* Concurrency: **Update locking on subgraphs, concurrent non-blocking ingest**, 
* Misc: **Free for Qualified Startups**.

#### [Sparksee](http://www.sparsity-technologies.com/dex.php)
* API: **Java, .NET, C++, Python, Objective-C, Blueprints Interface** 
* Protocol: **Embedded**, 
* Query Method: **as above + Gremlin (via Blueprints)**, 
* Written in: **C++**, Data Model: **Labeled Directed Attributed Multigraph**, 
* Concurrency: **yes**, 
* Misc: ACID possible, Free community edition up to 1 Mio objects, 
* Links: Intro [»](http://www.slideshare.net/SparsityTechnologies/sparksee-overview), Technical Overview [»](http://www.slideshare.net/SparsityTechnologies/sparksee-technology-overview)

#### [TITAN](https://github.com/thinkaurelius/titan/wiki)
* API: **Java, Blueprints, Gremlin, Python, Clojure** 
* Protocol: **Thrift, RexPro(Binary), Rexster (HTTP/REST)** 
* Query Method: **Gremlin, SPARQL** 
* Written In: **Java** Data Model: **labeled Property Graph, directed, multi-graph adjacency list** 
* Concurrency: **ACID Tunable C** 
* Replication: **Multi-Master** License: **Apache 2** Pluggable backends: **Cassandra, HBase, MapR M7 Tables, BDB, Persistit, Hazelcast** 
* Links: [Titan User Group](https://groups.google.com/forum/#!forum/aureliusgraphs)

#### [InfoGrid](http://infogrid.org/)
* API: **Java, http/REST**, 
* Protocol: **as API + XPRISO, OpenID, RSS, Atom, JSON, Java embedded**, 
* Query Method: **Web user interface with html, RSS, Atom, JSON output, Java native**, 
* Replication: **peer-to-peer**, 
* Written in: **Java,** 
* Concurrency: **concurrent reads, write lock within one MeshBase**, 
* Misc: Presentation [»](http://www.slideshare.net/infogrid/info-grid-core-ideas)

#### ~~[HyperGraphDB](http://www.kobrix.com/hgdb.jsp)~~
* API: **Java** (and Java Langs), 
* Written in:**Java**, 
* Query Method: **Java** or P2P, 
* Replication: **P2P**, 
* Concurrency: **STM**, 
* Misc: Open-Source, Especially for AI and Semantic Web.

#### [GraphBase](http://graphbase.net/)
Sub-graph-based API, query language, tools & transactions.  
Embedded Java, remote-proxy Java or REST.  
Distributed storage & processing. Read/write all Nodes. Permissions & Constraints frameworks. Object storage, vertex-embedded agents. Supports multiple graph models. 
* Written in Java

#### [Trinity](http://research.microsoft.com/en-us/projects/trinity/)
* API: **C#**, 
* Protocol: **C# Language Binding**, 
* Query Method: **Graph Navigation API**, 
* Replication: **P2P with Master Node**, 
* Written in: **C#**, 
* Concurrency: **Yes (Transactional update in online query mode, Non-blocking read in Batch Mode)** 
* Misc: **distributed in-memory storage, parallel graph computation platform (Microsoft Research Project)**

#### [AllegroGraph](http://www.franz.com/agraph/)
* API: **Java, Python, Ruby, C#, Perl, Clojure, Lisp** 
* Protocol: **REST**, 
* Query Method: **SPARQL** and **Prolog**, Libraries: **Social Networking Analytics** & **GeoSpatial**, 
* Written in: **Common** **Lisp**, 
* Links: Learning Center [»](http://www.franz.com/agraph/support/learning/), Videos [»](http://www.franz.com/agraph/services/conferences_seminars/)

#### [BrightstarDB](http://www.brightstardb.com/)
A native, **.NET**, semantic web database with code first Entity Framework, **LINQ** and **OData** support.   
* API: **C#**, 
* Protocol: **SPARQL HTTP, C#**, 
* Query Method: **LINQ, SPARQL**, 
* Written in: **C#**

#### ~~[Bigdata](http://www.systap.com/bigdata.htm)~~
* API: **Java, Jini service discovery**, 
* Concurrency: **very high (MVCC)**, 
* Written in: **Java**, 
* Misc: GPL + commercial, Data: **RDF data with inference, dynamic key-range sharding of indices**, 
* Misc: Blog [»](http://www.bigdata.com/blog) (parallel database, high-availability architecture, immortal database with historical views)

#### [Meronymy](http://www.meronymy.com/)
RDF enterprise database management system. It is cross-platform and can be used with most programming languages. Main features: high performance, guarantee database transactions with ACID, secure with ACL's, SPARQL & SPARUL, ODBC & JDBC drivers, RDF & RDFS. [»](http://en.wikipedia.org/wiki/Meronymy_SPARQL_Database_Server)

#### ~~[WhiteDB](http://whitedb.org/)~~
WhiteDB is a fast lightweight graph/N-tuples shared memory database library 
* written in C with focus on speed, portability and ease of use. Both for Linux and Windows, dual licenced with GPLv3 and a free nonrestrictive royalty-free commercial licence.

#### ~~[Onyx Database](http://onyxdevtools.com/)~~
Graph/ORM high throughput database built in Java supports embedded, in-memory, and remote. Horizontal scalability through sharding, partitioning, 
* replication, and disaster recovery   
* API: **Java, REST via (Objective C, Android, etc...)**, 
* Protocol: **Java embedded/Binary Socket/REST**, 
* Query Method: **Persistence Manager/ORM**, 
* Replication: **Multicast**, 
* Written in: **Java**, 
* Concurrency: **Re-entrant read/write**, 
* Misc: **Free and Open Source. Commercial licensing available**

#### [OpenLink Virtuoso](http://www.openlinksw.com/)
**Hybrid** DBMS covering the following models: **Relational, Document, Graph**

#### ~~[VertexDB](http://www.dekorte.com/projects/opensource/vertexdb/)~~

#### [FlockDB](http://github.com/twitter/flockdb)

#### [weaver](http://weaver.systems/)
scalable, fast, consistent

#### [BrightstarDB](http://www.brightstardb.com/)

#### ~~[Execom IOG](http://iog.codeplex.com/)~~

#### [Fallen 8](http://www.fallen-8.com/)
[Github](https://github.com/cosh/fallen-8)

#### ? OpenRDF / Sesame
#### ? Filament
#### ? OWLim
#### ? NetworkX
#### ? iGraph
#### ? Jena

### Multimodel Databases

#### [ArangoDB](https://www.arangodb.com/)
* API: **REST, Graph Blueprints, C#, D, Ruby, Python, Java, PHP, Go, Python, etc.** Data Model: **Documents, Graphs and Key/Values.** 
* Protocol: **HTTP using JSON.** 
* Query Method: **declarative query language (AQL), query by example.** 
* Replication: **master-slave (m-m to follow)**, Sharding: **automatic and configurable** 
* Written in: **C/C++/Javascript (V8 integrated),** 
* Concurrency: **MVCC, tuneable** 
* Misc: **ACID transactions, microservices framework "Foxx" (Javascript)**, many indices as **secondary, fulltext, geo, hash, Skip-list, capped collections**

#### [OrientDB](http://orientdb.com/)
* API: **REST, Binary Protocol, Java, Node.js, Tinkerpop Blueprints, Python, PHP, Go, Elixir, etc.**, Schema: **Has features of an Object-Database, DocumentDB, GraphDB and Key-Value DB**, 
* Written in: **Java**, 
* Query Method: **SQL, Gremlin, SparQL**, 
* Concurrency: **MVCC, tuneable**, Indexing: **Primary, Secondary, Composite indexes with support for Full-Text and Spatial**, 
* Replication: **Master-Masterfathomdb + sharding**, 
* Misc: **Really fast, Lightweight, ACID with recovery**.

#### ~~[FoundationDB](http://www.foundationdb.com/)~~
Bought by Apple Inc. Closed and reopened for public access.

#### [Datomic](http://www.datomic.com/)
* API: **Many jvm languages**, 
* Protocol: **Native + REST**, 
* Query Method: **Datalog + custom extensions**, Scaling: **elastic via underlying DB (in-mem, DynamoDB, Riak, CouchBase, Infinispan, more to come)**, 
* Written in: **Clojure**, 
* Concurrency: **ACID** 
* MISC: smart caching, unlimited read scalability, full-text search, cardinality, bi-directional refs 4 graph traversal, loves Clojure + Storm.

#### [gunDB](http://gundb.io/)
* API: **JavaScript** Schema: **Has features of an Object-Database, DocumentDB, GraphDB and Key-Value DB** 
* Written in: **JavaScript** 
* Query Method: **JavaScript** 
* Concurrency: **Eventual consistency with hybrid vector/timestamp/lexical conflict resolution** Indexing: **O(1) key/value, supports multiple indices per record** 
* Replication: **Multi-Master/Master; browser peer-to-peer (P2P) enabled** 
* Misc: **Open source, realtime sync, offline-first, distributed/decentralized, graph-oriented, and fault-tolerant**

#### [CortexDB](http://cortex-ag.com/cortexdoku/cms.php?i=206df578da20202020212024)
CortexDB is a dynamic schema-less multi-model data base providing nearly all advantages of up to now known NoSQL data base types (key-value store, document store, graph DB, multi-value DB, column DB) with dynamic re-organization during continuous operations, managing analytical and transaction data for agile software configuration,change requests on the fly, self service and low footprint.

#### [Oracle NOSQL Database](http://www.oracle.com/technetwork/database/database-technologies/nosqldb/overview/index.html)
Oracle NoSQL Database is a distributed key-value database with support for JSON docs. It is designed to provide highly reliable, scalable and available data storage across a configurable set of systems that function as storage nodes. NoSQL and the Enterprise Data is stored as key-value pairs, which are 
* written to particular storage node(s), based on the hashed value of the primary key. Storage nodes are replicated to ensure high availability, rapid failover in the event of a node failure and optimal load balancing of queries.   
* API: Java/C, Python, NodeJs, C#.

#### [AlchemyDB](http://code.google.com/p/alchemydatabase/)
GraphDB + RDBMS + KV Store + Document Store. Alchemy Database is a low-latency high-TPS NewSQL RDBMS embedded in the NOSQL datastore redis. Extensive datastore-side-scripting is provided via deeply embedded Lua. Bought and integrated with Aerospike.

#### [WonderDB](http://www.wonderdb.org/)
* API:JDBC,SQL; 
* WonderDB is fully transactional, distributed NewSQL database implemented in java based on relational architectures. 
  So you can get best of both worlds, sql, joins and ease of use from SQL and distribution, 
* replication and sharding from NoSQL movement. Tested performance is over 60K per node with Amazon m3.xlarge VM.

#### [RockallDB](http://www.rockallsoftware.com/)
A new concept to ‘NoSQL’ databases where a memory allocator and a transactional database are melted together into an almost seamless whole. 
The programming model uses variants of well-known memory allocation calls like ‘new’ and ‘delete’ to manage the database. 
The result is very fast, natural to use, reliable and scalable. 
It is especially good in Big Data, data collection, embedded, high performance, Internet of Things (IoT) or mobile type applications.

### [Object Databases](http://odbms.org/)

#### [Versant](http://www.versant.com/)
* API: Languages/
* Protocol: **Java, C#, C++, Python**. Schema: language class model (easy changable). Modes: **always consistent and eventually consistent** 
* Replication: **synchronous fault tolerant and peer to peer asynchronous**. 
* Concurrency: **optimistic and object based locks**. Scaling: **can add physical nodes on fly for scale out/in** **and migrate objects between nodes without impact to application code**. 
* Misc: **MapReduce via parallel SQL like query across logical database groupings.**

#### [db4o](http://db4o.com/)
* API: **Java, C#, .Net Langs**, 
* Protocol: **language**, 
* Query Method: **QBE (by Example), Soda, Native Queries, LINQ (.NET)**, 
* Replication: **db4o2db4o & dRS to relationals**, 
* Written in: **Java**, Cuncurrency: **ACID serialized**, 
* Misc: **embedded lib,** **Links**: DZone Refcard #53 [»](http://refcardz.dzone.com/refcardz/getting-started-db4o), Book [»](http://www.amazon.com/Definitive-Guide-db4o-Stefan-Edlich/dp/1590596560/),

#### [Objectivity](http://www.objectivity.com/)
* API: Languages: **Java, C#, C++, Python, Smalltalk, SQL access through ODBC**. Schema: **native language class model, direct support for references, interoperable across all language bindings. 64 bit unique object ID (OID) supports multi exa-byte**. Platforms: **32 and 64 bit Windows, Linux, Mac OSX, \*Unix**. Modes: **always consistent (ACID).** 
* Concurrency: **locks at cluster of objects (container) level.** Scaling: **unique distributed architecture, dynamic addition/removal of clients & servers, cloud environment ready.** 
* Replication: **synchronous with quorum fault tolerant across peer to peer partitions.**

#### [GemStone/S](http://gemtalksystems.com/)
* API: **Java, C, C++, Smalltalk** Schema: **language class model** Platforms: **Linux, AIX, Solaris, Mac OSX, Windows clients** Modes: **always consistent (ACID)** 
* Replication: **shared page cache per node, hot standby failover** 
* Concurrency: **optimistic and object based locks** Scaling: **arbitrarily large number of nodes** 
* Misc: **SQL via GemConnect**

#### [Starcounter](http://www.starcounter.com/)
* API: **C# (.NET languages)**, Schema: **Native language class model**, 
* Query method: **SQL**, 
* Concurrency: **Fully ACID compliant**, Storage: **In-memory with transactions secured on disk**, Reliability: **Full checkpoint recovery**, 
* Misc: **VMDBMS - Integrating the DBMS with the virtual machine for maximal performance and ease of use**.

#### [Perst](http://www.mcobject.com/perst)
* API: **Java,Java ME,C#,Mono**. 
* Query method: **OO via Perst collections, QBE, Native Queries, LINQ, native full-text search, JSQL** 
* Replication: **Async+sync (master-slave)** 
* Written in: **Java, C#**. Caching: **Object cache (LRU, weak, strong), page pool, in-memory database** 
* Concurrency: **Pessimistic+optimistic (MVCC)** **\+ async or sync (ACID)** Index types: **Many tree models + Time Series**. 
* Misc: Embedded lib., encryption, automatic recovery, native full text search, on-line or off-line backup.

#### [VelocityDB](http://www.velocitydb.com/)
* Written in**100% pure C#**, 
* Concurrency: **ACID/transactional, pessimistic/optimistic locking,** 
* Misc: compact data, B-tree indexes, LINQ queries, 64bit object identifiers (Oid) supporting multi millions of databases and high performance. Deploy with a single DLL of around 400KB.

#### ~~[HSS Database](http://highspeed-solutions.net/db.aspx)~~
* Written in: **100% C#**, The HSS DB v3.0 (HighSpeed-Solutions Database), is a client based, zero-configuration, auto schema evolution, acid/transactional, LINQ Query, DBMS for Microsoft .NET 4/4.5, Windows 8 (Windows Runtime), Windows Phone 7.5/8, Silverlight 5, MonoTouch for iPhone and Mono for Android

#### [ZODB](http://zodb.org/)
* API: **Python**, 
* Protocol: **Internal, ZEO**, 
* Query Method: **Direct object access, zope.catalog, gocept.objectquery,** 
* Replication: **ZEO, ZEORAID, RelStorage** 
* Written in: **Python, C** 
* Concurrency: **MVCC**, License: **Zope Public License (OSI approved)** 
* Misc: Used in production since 1998

#### [Newt DB](http://www.newtdb.org/)
Newt DB leverages the pluggable storage layer of ZODB to use RelStorage to store data in Postgres. Newt adds conversion of data from the native serialization used by ZODB to JSON, stored in a Postgres JSONB column. The JSON data supplements the native data to support indexing, search, and access from non-Python applications. It adds a search API for searching the Postgres JSON data and returning persistent objects. 
* Query Method: **Postgres SQL, ZODB API**, 
* Replication: **Postgres, ZEO, ZEORAID, RelStorage**, 
* Written in: **Python**, 
* Concurrency: **MVCC**, License: **MIT**

#### [NEO](http://www.neoppod.org/)
* API: **Python - ZODB "Storage" interface**, 
* Protocol: **native**, 
* Query Method: **transactional key-value**, 
* Replication: **native**, 
* Written in: **Python**, 
* Concurrency: **MVCC** (internally), License: **GPL "v2 or later"**, 
* Misc: Load balancing, fault tolerant, hot-extensible.

#### [Magma](http://wiki.squeak.org/squeak/2665)
Smalltalk DB, optimistic locking, Transactions, etc.

#### [siaqodb](http://siaqodb.com/)
An object database engine that currently runs on **.NET, Mono, Silverlight,Windows Phone 7, MonoTouch, MonoAndroid, CompactFramework**; It has implemented a Sync Framework Provider and can be **synchronized** with **MS SQLServer**; 
* Query method:**LINQ**;

#### [JADE](https://www.jadeworld.com/)
Programming Language with an Object Database build in. Around since 1996.

#### ~~[Sterling](http://sterling.codeplex.com/)~~
is a lightweight object-oriented database for .NET with support for Silverlight and Windows Phone 7. It features in-memory keys and indexes, triggers, and support for compressing and encrypting the underlying data.

#### [MarcelloDB](https://github.com/markmeeus/MarcelloDB)
An embedded object database designed for **mobile apps** targetting .net and Mono runtimes. Supports .net/mono, Xamarin (iOS and Android), Windows 8.1/10, Windows Phone 8.1. Simple API, built on top of json.net and has a simple but effective indexing mechanism. Development is focussed on being lightweight and developer friendly. Has transaction support. Open-source and free to use.

#### [Morantex](http://www.morantex.com/)
Stores .NET classes in a datapool. Build for speed. SQL Server integration. LINQ support.

#### [EyeDB](http://www.eyedb.org/)
EyeDB is an **LGPL OODBMS**, provides an advanced object model (inheritance, collections, arrays, methods, triggers, constraints, reflexivity), an object definition language based on ODMG ODL, an object query and manipulation language based on ODMG OQL. Programming interfaces for C++ and Java.

#### [FramerD](http://www.framerd.org/)
Object-Oriented Database designed to support the maintenance and sharing of knowledge bases. Optimized for pointer-intensive data structures used by semantic networks, frame systems, and many intelligent agent applications. 
* Written in: **ANSI C**.

#### ~~[Ninja Database Pro](http://www.kellermansoftware.com/p-43-ninja-net-database-pro.aspx)~~
Ninja Database Pro is a .NET ACID compliant relational object database that supports transactions, indexes, encryption, and compression. It currently runs on .NET Desktop Applications, Silverlight Applications, and Windows Phone 7 Applications.

#### [NDatabase](http://ndatabase.wix.com/home)
* API: **C#, .Net, Mono, Windows Phone 7, Silverlight,** 
* Protocol: **language,** 
* Query Method: **Soda, LINQ (.NET),** 
* Written in: **C#**, 
* Misc: embedded lib, indexes, triggers, handle circular ref, LinqPad support, Northwind sample, refactoring, in-memory database, Transactions Support (ACID) 

#### [PicoLisp](http://www.picolisp.com/)
Language and Object Database, can be viewed as a Database Development Framework. Schema: **native language class model with relations + various indexes**. Queries: **language build in + a small Prolog like DSL Pilog**. 
* Concurrency: **synchronization + locks**. 
* Replication, distribution and fault tolerance is not implemented per default but can be implemented with native functionality. 
* Written in C (32bit) or assembly (64bit).

#### [acid-state](http://acid-state.seize.it/)
* API: **Haskell**, 
* Query Method: **Functional programming**, 
* Written in: **Haskell**, 
* Concurrency: **ACID, GHC concurrent runtime**, 
* Misc: In-memory with disk-based log, supports remote access 
* Links: Wiki [»](http://acid-state.seize.it/), Docs [»](http://hackage.haskell.org/packages/archive/acid-state/0.8.3/doc/html/Data-Acid.html)

#### [ObjectDB](http://www.objectdb.com/)
* API: **Java (JPA / JDO)** 
* Query method: **JPA JPQL, JDO JDOQL** 
* Replication: **Master-slave** 
* Written in: **100% Pure Java** Caching: **Object cache, Data cache, Page cache, Query Result cache, Query program cache** 
* Concurrency: **Object level locking (pessimistic + optimistic)** Index types: **BTree, single, path, collection** 
* Misc: Used in production since 2004, Embedded mode, Client Server mode, automatic recovery, on-line backup.

#### [CoreObject](http://www.coreobject.org/)
CoreObject: Version-controlled OODB, that supports powerful undo, semantic merging, and real-time collaborative editing. MIT-licensed,   
* API: **ObjC**, Schema: **EMOF-like**, 
* Concurrency: **ACID**, 
* Replication: **differential sync**, 
* Misc: DVCS based on object graph diffs, selective undo, refs accross versioned docs, tagging, temporal indexing, integrity checking.

#### [StupidDB](http://www.aztec-project.org/)

#### [KiokuDB](http://www.iinteractive.com/kiokudb/)

#### [Durus](http://www.mems-exchange.org/software/durus/)

### Grid & Cloud Database Solutions

#### [GridGain](https://www.gridgain.com/)
In-Memory Computing Platform built on Apache® Ignite™ to provide high-speed transactions with ACID guarantees, real-time streaming, and fast analytics in a single, comprehensive data access and processing layer. The distributed in-memory key value store is ANSI SQL-99 compliant with support for SQL and DML via JDBC or ODBC.   
API: Java, .NET, and C++. Minimal or no modifications to the application or database layers for architectures built on all popular RDBMS, NoSQL or Apache™ Hadoop® databases.

#### [Crate Data](https://crate.io/)
[shared nothing](http://en.wikipedia.org/wiki/Shared_nothing_architecture), document-oriented cluster 
* data store. Accessed via SQL and has builtin [BLOB support](https://crate.io/blog/using-crate-data-as-a-blobstore/). Uses the cluster state implementation and node discovery of Elasticsearch. License: **Apache 2.0**, 
* Query Method: **SQL**, Clients: **HTTP (REST), Python, Java (JDBC or native), Ruby, JS, Erlang**, 
* Replication + Sharding: **automatic and configurable**, 
* written in: **Java**, [» Crate Data GitHub Project](https://github.com/crate/crate), [» Documentation](https://crate.io/docs/).

#### [Oracle Coherence](http://www.oracle.com/technetwork/middleware/coherence/overview/index.html)
Oracle Coherence offers distributed, replicated, multi-datacenter, tiered (off-heap/SSD) and near (client) caching. It provides distributed processing, querying, eventing, and map/reduce, session management, and prorogation of database updates to caches. Operational support provided by a Grid Archive deployment model.

#### [GigaSpaces](http://www.gigaspaces.com/)
Popular SpaceBased Grid Solution.

#### ~~[GemFire](http://www.vmware.com/products/application-platform/vfabric-gemfire)~~
GemFire offers in-memory globally distributed data management with dynamic scalability, very high performance and granular control supporting the most demanding applications. Well integrated with the Spring Framework, developers can quickly and easily provide sophisticated data management for applications. With simple horizontal scale-out, data latency caused by network roundtrips and disk I/O can be avoided even as applications grow.

#### [Infinispan](http://www.jboss.org/infinispan.html)
scalable, highly available data grid platform, **open source**, 
* written in **Java**.

#### ~~[Queplix](http://www.queplix.com/)~~
NOSQL Data Integration Environment, can integrate relational, object, BigData – NOSQL easily and without any SQL.

#### [Hazelcast](http://www.hazelcast.com/)
Hazelcast is a in-memory data grid that offers distributed data in Java with dynamic scalability under the Apache 2 open source license. It provides distributed data structures in Java in a single Jar file including hashmaps, queues, locks, topics and an execution service that allows you to simply program these data structures as pure java objects, while benefitting from symmetric multiprocessing and cross-cluster shared elastic memory of very high ingest data streams and very high transactional loads.

#### [ScaleOut Software](http://www.scaleoutsoftware.com/)
#### [Galaxy](http://puniverse.github.com/galaxy/)
#### [Joafip](http://joafip.sourceforge.net/)
#### ? Coherence
#### ? eXtremeScale

### XML Databases

#### [EMC Documentum xDB](http://www.emc.com/products/detail/software/documentum-xdb.htm)
* API: Java, XQuery, 
* Protocol: WebDAV, web services, 
* Query method: XQuery, XPath, XPointer, 
* Replication: lazy primary copy 
* replication (master/replicas), 
* Written in: Java, 
* Concurrency: concurrent reads, writes with lock; transaction isolation, 
* Misc: Fully transactional persistent DOM; versioning; multiple index types; metadata and non-XML data support; unlimited horizontal scaling

#### [eXist](http://exist-db.org/)
* API: XQuery, XML:DB API, DOM, SAX, 
* Protocols: HTTP/REST, WebDAV, SOAP, XML-RPC, Atom, 
* Query Method: XQuery, 
* Written in: Java (open source), 
* Concurrency: Concurrent reads, lock on write; 
* Misc: Entire web applications can be 
* written in XQuery, using XSLT, XHTML, CSS, and Javascript (for AJAX functionality). (1.4) adds a new full text search index based on Apache Lucene, a lightweight URL rewriting and MVC framework, and support for XProc.

#### ~~[Sedna](http://modis.ispras.ru/sedna/)~~
* Misc: ACID transactions, security, indices, hot backup. Flexible XML processing facilities include W3C XQuery implementation, tight integration of XQuery with full-text search facilities and a node-level update language.

#### [BaseX](http://basex.org/)
BaseX is a fast, powerful, lightweight XML database system and XPath/XQuery processor with highly conformant support for the latest W3C Update and Full Text Recommendations. Client/Server architecture, ACID transaction support, user management, logging, Open Source, BSD-license, 
* written in Java, runs out of the box.

#### [fathomdb](http://www.fathomdb.com/)

#### ~~[Qizx](http://www.xmlmind.com/qizx/)~~
commercial and open source version,   
* API: Java, 
* Protocols: HTTP, REST, 
* Query Method: XQuery, XQuery Full-Text, XQuery Update, 
* Written in: Java, full source can be purchased, 
* Concurrency: Concurrent reads & writes, isolation, 
* Misc: Terabyte scalable, emphasizes query speed.

#### [Berkeley DB XML](http://www.oracle.com/database/berkeley-db/xml/index.html) 
* API: Many languages, 
* Written in: C++, 
* Query Method: XQuery, 
* Replication: Master / Slave, 
* Concurrency: MVCC, License: Sleepycat  

#### ~~[JEntigrator](http://www.gradatech.de/whitepaper.html)~~
API: Java. The application and database management system in one. Collects data as multiple XML files on the disk. Implements facet-oriented data model. Each data object is considered as an universal facet container. The end-user can design and evolve data objects individually through the GUI without any coding by adding/removing facets to/from it.

#### [Xindice](http://xml.apache.org/xindice/)

#### [Tamino](http://www.softwareag.com/de/products/wm/tamino/)

### Multidimensional Databases

#### [Globals](http://globalsdb.org/)
by Intersystems, multidimensional array.Node.js API, array based APIs (Java / .NET), and a Java based document API.

#### [Intersystems Cache](http://www.intersystems.com/)
Postrelational System. Multidimensional array APIs, Object APIs, Relational Support (Fully SQL capable JDBC, ODBC, etc.) and Document APIs are new in the upcoming 2012.2.x versions. Availible for Windows, Linux and OpenVMS.

#### ~~[GT.M](http://fis-gtm.com/)~~
* API: **M, C, Python, Perl**, 
* Protocol: **native, inprocess C**, 
* Misc: Wrappers: **M/DB for SimpleDB compatible HTTP** 
* [»](http://www.mgateway.com/mdb.html), 
* **MDB:X** for XML [»](http://mgateway.com/), 
* **PIP** for mapping to tables for SQL [»](http://fis-pip.com/), 
* Features: Small footprint (17MB), 
* Terabyte Scalability, 
* Unicode support, 
* Database encryption, Secure, 
* ACID transactions (single node), 
* eventual consistency ( replication)
* **Links**: Slides [»](http://www.slideshare.net/robtweed/gtm-a-tried-and-tested-schemaless-database),

#### ~~[SciDB](http://scidb.org/)~~
**Array** Data Model for Scientists, 
[» HiScaBlog](http://highscalability.com/blog/2010/4/29/product-scidb-a-science-oriented-dbms-at-100-petabytes.html)

#### [MiniM DB](http://www.minimdb.com/)
: Multidimensional arrays,   
* API: **M, C, Pascal, Perl, .NET, ActiveX, Java, WEB.** Available for Windows and Linux.

#### [rasdaman](http://www.rasdaman.org/)
: Short description: Rasdaman is a scientific database that allows to store and retrieve multi-dimensional raster data (arrays) of unlimited size through an SQL-style query language.   
* API: C++/Java, 
* Written in C++, 
* Query method: SQL-like query language rasql, as well as via OGC standards WCPS, WCS, WPS [link2](http://www.rasdaman.com/)

#### [DaggerDB](http://www.daggerdb.com/)
is a new Realtime analytics database 
* written in .NET C#. 
* ACID compliant. 
* fluent .NET query API, Client / server or in-process. 
* In-memory and persistent mode.

#### [EGTM: GT.M for Erlang](https://github.com/ztmr/egtm) 

#### [IODB: EGTM-powered ObjectDB for Erlang](http://www.idea.cz/technology)

### Multivalue Databases

#### [U2](http://www.rocketsoftware.com/u2)
(UniVerse, UniData): MultiValue Databases, Data Structure: MultiValued, Supports nested entities, Virtual Metadata,   
* API: BASIC, InterCall, Socket, .NET and Java API's, IDE: Native, Record Oriented, Scalability: automatic table space allocation, 
* Protocol: Client Server, SOA, Terminal Line, X-OFF/X-ON, 
* Written in: C, 
* Query Method: Native mvQuery, (Retrieve/UniQuery) and SQL, 
* Replication: yes, Hot standby, 
* Concurrency: Record and File Locking (Fine and Coarse Granularity)

#### [OpenInsight](http://www.revelation.com/index.php/features)
  
* API: Basic+, .Net, COM, Socket, ODBC, 
* Protocol: TCP/IP, Named Pipes, Telnet, VT100. HTTP/S 
* Query Method: RList, SQL & XPath 
* Written in: Native 4GL, C, C++, Basic+, .Net, Java 
* Replication: Hot Standby 
* Concurrency: table &/or row locking, optionally transaction based & commit & rollback Data structure: Relational &/or MultiValue, supports nested entities Scalability: rows and tables size dynamically

#### ~~[TigerLogic PICK](http://www.tigerlogic.com/tigerlogic/pick/database/pickdatabase.jsp)~~
D3, mvBase, mvEnterprise Data Structure: 
**Dynamic multidimensional PICK data model, multi-valued, dictionary-driven**,   
* API: **NET, Java, PHP, C++,** 
* Protocol: **C/S**, 
* Written In: **C**, 
* Query Method: **AQL, SQL, ODBC, Pick/BASIC**, 
* Replication: **Hot Backup, FFR, Transaction Logging + real-time 
* replication**, 
* Concurrency: **Row Level Locking**, Connectivity: **OSFI, ODBC, Web-Services, Web-enabled**, Security: File level AES-128 encryption

#### [Reality](http://www.nps-reality.com/)
(Reality NPS): The original MultiValue dataset database, virtual machine, enquiry and rapid development environment. Delivers ultra efficiency, scalability and resilience while extended for the web and with built-in auto sizing, failsafe and more. Interoperability includes Web Services - Java Classes, RESTful, XML, ActiveX, Sockets, .NetLanguages, C and, for those interoperate with the world of SQL, ODBC/JDBC with two-way transparent SQL data access.

#### [OpenQM](http://www.openqm.com/)
Supports nested data. Fully automated table space allocation. 
* Concurrency control via task locks, file locks & shareable/exclusive record locks. Case insensitivity option. Secondary key indices. Integrated data 
* replication. QMBasic programming language for rapid development. OO programming integrated into QMBasic. QMClient connectivity from Visual Basic, PowerBasic, Delphi, PureBasic, ASP, PHP, C and more. Extended multivalue query language.

#### [Model 204 Database](http://www.rocketsoftware.com/m204)
A high performance dbms that runs on IBM mainframes (IBM z/OS, z/VM, zVSE), +SQL interface with nested entity support   
* API: native 4GL (SOUL + o-o support), SQL, Host Language (COBOL, Assembler, PL1) API, ODBC, JDBC, .net, Websphere MQ, Sockets Scalability: automatic table space allocation, 64-bit support 
* Written in: IBM assembler, C 
* Query method: SOUL, SQL, RCL ( invocation of native language from client ) 
* Concurrency: record and file level locking Connectivity: TN3270, Telnet, Http

#### [Tieto TRIP](http://www.tieto.com/services/information-management/enterprise-content-management-services/multimedia-content-management-trip)
Hybrid database / search engine system with characteristics of multi-value, document, relational, XML and graph databases. Used in production since 1985 for high-performance search and retrieve solutions. Full-text search, text classification, similarity search, results ranking, real time facets, Unicode, Chinese word segmentation, and more. Platforms: Windows, Linux, AIX and Solaris.   
* API: .NET, Java and C/C++. 
* Query methods: native (CCL), SQL subset, XPath. Commercial.

#### [ESENT](http://msdn.microsoft.com/en-us/library/windows/desktop/gg269259%28v=EXCHG.10%29.aspx)
(by Microsoft) ISAM storage technology. Access using index or cursor navigation. Denormalized schemas, wide tables with sparse columns, multi-valued columns, and sparse and rich indexes. C# and Delphi drivers available. Backend for a number of MS Products as Exchange.

#### [jBASE](http://www.jbase.com/index.html)
jBASE is an application platform and database that allows normal MultiValue (MV) applications to become native Windows, Unix or Linux programs. Traditional MV features are supported, including BASIC, Proc, Paragraph, Query and Dictionaries. jBASE jEDI Architecture, allows you to store data in any database, such as Microsoft SQL, Oracle and DB2. jBASE jAgent supports BASIC, C, C++, .NET, Java and REST APIs. Additional features include dynamic objects, encryption, case insensitivity, audit logging and transaction journaling for online backup and disaster recovery.

### Event Sourcing

#### [Event Store](http://geteventstore.com/)

#### [Eventsourcing for Java (es4j)](http://eventsourcing.com/)
* Clean, 
* succinct Command/Event model, 
* Compact data storage layout, 
* Disruptor for fast message processing, 
* CQengine for fast indexing and querying, 
* In-memory and on-disk storage, 
* Causality-preserving Hybrid Logical Clocks, 
* Locking synchronization primitive, 
* OSGi support

### Time Series / Streaming Databases

#### [Axibase](http://axibase.com/products/axibase-time-series-database/)
Distributed DB designed to store and analyze high-frequency time-series data at scale. Includes a large set of built-in features: Rule Engine, Visualization, Data Forecasting, Data Mining.   
* API: **RESTful API, Network API, Data API, Meta API, SQL** API Clients: **R, Java, Ruby, Python, PHP, Node.js** 
* Replication: **Master Slave** Major protocol & format support: **CSV, nmon, pickle, StatsD, collectd, tcollector, scollector, JMX, JDBC, SNMP, JSON, ICMP, OVPM, SOAP**.

#### [kdb+](http://kx.com/)
time-series database optimized for Big Data analytics.

#### [quasardb](https://www.quasardb.net/)
very high-performance time series database. Highly scalable.   
* API: **C, C++, Java, Python and (limited) RESTful** 
* Protocol: **binary** 
* Query method: **API/SQL-like,** 
* Replication: **Distributed**, 
* Written in: **C++ 11/Assembly**, 
* Concurrency: **ACID**, 
* Misc: **built-in data compression, native support for FreeBSD, Linux and Windows**. License: **Commercial**.

#### [Riak TS](http://basho.com/products/riak-ts/)
Enterprise-grade NoSQL time series database optimized specifically for IoT and Time Series data. It ingests, transforms, stores, and analyzes massive amounts of time series data. Riak TS is engineered to be faster than Cassandra.

#### [Informix Time Series Solution](https://www.ibm.com/developerworks/data/library/techarticle/dm-1203timeseries/index.html)

#### [influxdata](https://influxdata.com/)

#### [pipelinedb](https://pipelinedb.com/)

#### [eXtremeDB](http://www.mcobject.com/extremedbfamily.shtml)
(listed also under other NoSQLrelated DBs)

### Other NoSQL related databases

#### [IBM Lotus/Domino](http://www-01.ibm.com/software/lotus/)
Type: Document Store,   
* API: Java, HTTP, IIOP, C API, REST Web Services, DXL, Languages: Java, JavaScript, LotusScript, C, @Formulas, 
* Protocol: HTTP, NRPC, 
* Replication: Master/Master, 
* Written in: C, 
* Concurrency: Eventually Consistent, Scaling: 
* Replication Clusters

#### [eXtremeDB](http://www.mcobject.com/extremedbfamily.shtml)
Type: Hybrid In-Memory and/or Persistent Database Database; 
* Written in: C;   
* API: C/C++, SQL, JNI, C#(.NET), JDBC; 
* Replication: Async+sync (master-slave), Cluster; Scalability: 64-bit and MVCC

#### [RDM Embedded](http://www.raima.com/products/rdme/)
APIs: C++, Navigational C. Embedded Solution that is ACID Compliant with Multi-Core, On-Disk & In-Memory Support. Distributed Capabilities, Hot Online Backup, supports all Main Platforms. Supported B Tree & Hash Indexing. 
* Replication: Master/Slave, 
* Concurrency: MVCC. Client/Server: In-process/Built-in.

#### [ISIS Family](http://www.unesco.org/webworld/isis/isis.htm)

#### [Moonshadow](http://www.moonshadowmobile.com/data-visualization/big-data-visualizer/)
NoSql, in-memory, flat-file, cloud-based. API interfaces. Small data footprint and very fast data retrieval. Stores 200 million records with 200 attributes in just 10GB. Retrieves 150 million records per second per CPU core. Often used to visualize big data on maps. 
* Written in C.

#### ~~[VaultDB](http://www.rediosoft.com/)~~
Next-gen NoSQL encrypted document store. Multi-recipient / group encryption. Featuers: 
* concurrency, indices, ACID transactions, 
* replication and PKI management. Supports PHP and many others. 
* Written in **C++**. Commercial but has a free version.   
* API: **JSON**

#### ~~[Vyhodb](http://www.vyhodb.com/)~~
**Service oriented, schema-less, network data model DBMS**. Client application invokes methods of vyhodb services, which are 
* written in Java and deployed inside vyhodb. Vyhodb services reads and modifies storage data.   
* API: **Java**, 
* Protocol: RSI - **Remote service invocation**, 
* Written in: Java, ACID: **fully supported**, 
* Replication: **async master slave**, 
* Misc: **online backup**, License: proprietary

#### [Applied Calculus](http://nncnannara.net/)
Applied Calculus implements persistent AVL Trees / AVL Databases. 14 different types of databases - represented as classes in both C# and Java. These databases perform transaction logging on the node file to ensure that failed transactions are backed out. Very fast on solid state storage (ca. 1780 transactions/second. AVL Trees considerably outperform B+ Trees on solid state. Very natural language interface. Each database is represented as a collection class that strongly resembles the corresponding class in Pure Calculus.

#### [Prevayler](http://www.prevayler.org/)
Java RAM Data structure journalling.

#### [Yserial](http://yserial.sourceforge.net/)
Python wrapper over sqlite3

#### ~~[SpreadsheetDB](https://www.spreadsheetdb.io/)~~
A database as a service that can be queried with a spreadsheet through an HTTP API.

### Scientific and Specialized DBs

#### ~~[BayesDB](http://probcomp.csail.mit.edu/bayesdb/)~~
BayesDB, a Bayesian database table, lets users query the probable implications of their tabular data as easily as an SQL database lets them query the data itself. Using the built-in Bayesian Query Language (BQL), users with no statistics training can solve basic data science problems, such as detecting predictive relationships between variables, inferring missing values, simulating probable observations, and identifying statistically similar database entries.

#### [GPUdb](http://www.gpudb.com/)
A distributed database for many core devices. 
GPUdb leverages many core devices such as NVIDIA GPUs to provide an unparallelled parallel database experience. 
GPUdb is a scalable, distributed database with SQL-style query capability, capable of storing Big Data. 
Developers using the GPUdb API add data, and query the data with operations like select, group by, and join. 
GPUdb includes many operations not available in other "cloud database" offerings.

### unresolved and uncategorized

#### [fathomdb](http://www.fathomdb.com/) 
moved to [Meteor](https://www.meteor.com/)

#### [Btrieve](http://www.wordiq.com/definition/Btrieve)
key/index/tuple DB. Using Pages. 

#### [KirbyBase](https://rubygems.org/gems/KirbyBase)
* Written in: **Ruby**. github: [»](https://github.com/gurugeek/KirbyBase)

#### ~~[Tokutek](http://tokutek.com/)~~

#### [Recutils](http://www.gnu.org/software/recutils/)
GNU Tool for text files containing records and fields. Manual [»](http://www.gnu.org/software/recutils/manual/index.html)

#### ~~[FileDB](http://www.eztools-software.com/tools/filedb/)~~
Mainly targeted to Silverlight/Windows Phone developers but its also great for any .NET application where a simple local database is required, extremely Lightweight - less than 50K, stores one table per file, including index, compiled versions for Windows Phone 7, Silverlight and .NET, fast, free to use in your applications

#### [MentDB](http://www.mentdb.org/)
A digital brain, based on the language of thought (Mentalese), to manage relations and strategies (with thoughts/words/symbols) into a cognitive cerebral structure. 
* Programing language: **MQL** (Mentalese Query Language) for mental processes, 
* **ACID** transaction,   
* API: **REST** and **web** socket, Thoughts Performance: READ: 18035/s; WRITE: 416/s; Asynchronous Full **JAVA**

#### [CodernityDB](http://labs.codernity.com/codernitydb/)
* written in Python

#### **illuminate Correlation Database** [»](http://www.datainnovationsgroup.com/), 

#### **FluidDB (Column Oriented DB)** [»](http://doc.fluidinfo.com/fluidDB/), 

#### [Fleet DB](http://fleetdb.org/), 

#### [Btrieve](http://twistedstorage.com/), 

#### [Java-Chronicle](https://github.com/peter-lawrey/Java-Chronicle)

#### [Adabas](http://documentation.softwareag.com/adabas/ada814mfr/adamf/concepts/cfadais.htm)

## nosql list
* [db engines](https://db-engines.com/en/ranking)
* [nosql-database archive](https://sheinin.github.io/nosql-database.org/)
* [nosql-database archive ~2019](https://github.com/edlich/nosql-database.org)

## Theory
![NoSql](https://i.postimg.cc/qBmqcVD2/NoSql.png)
![NoSql BoltDB](https://i.postimg.cc/FRfS4fc9/nosql-boltdb.png)
