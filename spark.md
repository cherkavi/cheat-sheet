## Spark
[main documentation](https://spark.apache.org/docs/latest/)
[app examples](https://mvnrepository.com/artifact/org.apache.spark/spark-examples)
[scala, java, python start point app](https://courses.cognitiveclass.ai/asset-v1:BigDataUniversity+BD0211EN+2016+type@asset+block/Exercise_3.pdf)
[configuration and monitoring](https://courses.cognitiveclass.ai/asset-v1:BigDataUniversity+BD0211EN+2016+type@asset+block/Exercise_5.pdf)

### configuration ( spark-defaults.conf )
http://<driver>:4040
	
* spark properties
```
new SparkConf().set("spark.executor.memory","1g")
```
```
bin/spark-submit --conf "spark.executor.extraJavaOptions=-XX:+PrintGCDetails -XX:+PrintGCTimeStamps"
```
* conf/spark-env.sh
* log4j.properties

```
spark.rdd.compress=false
spark.shuffle.consolidateFiles=false # set true for ext4/xfs filesystems

spark.shuffle.spill=true
spark.shuffle.memoryFraction=0.2 # memory limit used during reduce by spilling on disk
spark.shuffle.spill.compress=true

spark.storage.memoryFraction=0.6 # how much storage will be dedicated to in-memory storage

spark.storage.unrollFraction=0.2 # unrolling serialized data
```


### metrics
Coda Hale Metrics Library
/conf/metrics.properties

### history server
/sbin/start-history-server.sh

### serialization
* java 
* kyro
conf.set("spark.serializer", "org.apache.spark.serializer.KryoSerializer")
conf.registerKryoClasses(Array(classOf[MyOwnClass1], classOf[MyOwnClass2]))
conf.set("spark.scheduler.allocation.file", "/path/to/allocations.xml")
### instrumentations
* Ganglia
* OS profiling tools
* JVM utilities

using [Alluxio](alluxio.org) to save/read RDD across SparkContext

### packages
java/scala packages
```
<dependency>
    <groupId>org.apache.spark</groupId>
    <artifactId>spark-core_2.10</artifactId>
    <version>2.2.2</version>
</dependency>
```
```
org.apache.spark.SparkContext.*
org.apache.spark.SparkConf
org.apache.spark.api.java.JavaRDD
org.apache.spark.api.java.JavaSparkContext
```
python
```
from pyspark import SparkContext, SparkConf
```

### context
scala
```
val sparkConfig = new SparkConf().setAppName("my-app").setMaster("local[4]")
val sc = new SparkContext(  )
val sqlContext = new org.apache.spark.sql.SQLContext(sc)
val streamingContext = new StreamingContext(conf, Seconds(1))
```
java
```
sc = new JavaSparkContext( new SparkConf().setAppName("my-app").setMaster("local[*]") )

new org.apache.spark.sql.api.java.SQLContext(sc)
```
python
```
sc = SparkContext( conf = SparkConf().setAppName("my-app").setMaster("local") )

from pyspark.sql import SQLContext
sqlContext = SQLContext(sc)
```

### Side effect
any task can be executed more than once !!!!
```
# default value for failing of tasks
spark.task.maxFailures=4
```

### slow tasks re-run, re-start of slow task
```
# activate re-launch
spark.speculation = true
# when task is considered like 'slow', when execution time slower than ... median of all executions
spark.speculation.multiplier = 1.5
```

### schedule mode
```
# default = FIFO
spark.scheduler.mode = FAIR # not to wait in the queue - round robin eviction to be executed somewhere
```

### passing functions into Spark
* anonymous functions syntax
```
(a: Int) => a + 1
```
* static method
```
object FunctionCollections{
	def sizeCounter(s: String): Int = s.length
}
currentRdd.map(FunctionsCollections.sizeCounter)
```
* by reference
```
def myExecutor(rdd: RDD[String]): RDD[String] = {... rdd.map(a=> a+1) }
```

### useful functions
```
val data = sc.textFile("some_data.csv")

// debug information of RDD
data.toDebugString()

// descriptions of partitions
data.partitions

// statistic values
.stats()


// histogram, mean, stdev, sum, variance, min, max
```

## Serialization
### serialization issue
any class, that involved into Task should be Serializable
```
class HelperForString extends Serialization{
}
```
using lazy initialization for any object inside ( instead of Serializable)
```
class HelperForString extends Serializable{
	@transient lazy val innerHelper = new AnotherHelperForString()
}
```

### serialization types
Java - default
Python - pickle
Scala - Kryo

## Persistence
good place for persistence
```
sc.textFile("data/*.csv").
filter(!_.startsWith("Trip ID")).map(_.split(",")). // skip first line
map(StaticClassUtility.parse(_)).
keyBy(_.column2).
partitionBy(new HashPartitioner(8)).
// after at least one 'heavy' action should be persisted
persist(StorageLevel.xxxxxxx)


rdd1.join(rdd2).
persist(StorageLevel.xxxxxxx)

```

### cache and persist
cache == persist(StorageLevel.MEMORY_ONLY)

### unpersist RDD
```
// should be called after "action"
rdd.unpersist()
```

### difference betwee reduceByKey and countByKey
```
val text = sc.textFile("my_own_file")
val counts = 
	// reduceByKey - reduce all values by key, after - combine the result for each partition
	text.flatMap(_.split(" ")).map((_, 1)).reduceByKey(_+_).collectAsMap()
	text.flatMap(_.split(" ")).countByValue()
// call reduce( _ + _ ), collect result from each partitions, sent to driver !!! ( only for testing/development )
.countByKey

```	

examples
```
// approximate result of large dataset:
.countApproxDistinct

// update accumulator with value and return it
.fold((accumulator, value)=> accumulator)

// group all values by key from all partitions into memory !!! ( Memory issue !!!)
.groupByKey

// return all values for specified key
.lookup

// apply map function to value only, avoid shuffling between partitions
.mapValues 

```

# libraries
* Spark SQL
```
val employee = sqlContext.sql("select employee.name, employee.age from employee where salary>120000")
employee.map(empl=> "Name:"+empl(0)+"  age:"+empl(1).sort(false).take(5).foreach(println)
```
* Spark Streaming
```
val lines = streamingContext.socketTextStream("127.0.0.1", 8888)
val words = lines.flatMap(_.split(" "))
words.map(w=>(w,1)) .reduceByKey(_ + _) .print()
```
* MLib
* GraphX
```
GraphLoader.edgeListFile(sc, path_to_file)
```



# Scala examples
```
val input = sc.textFile("data/stations/*")

val header = input.first // to skip the header row

val stations = input2.
	filter(_ != header2).
	map(_.split(",")).
	keyBy(_(0).toInt)

stations.join(trips.keyBy(_(4).toInt))
```