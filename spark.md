## Spark
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

### metrics
Coda Hale Metrics Library
/conf/metrics.properties

### history server
/sbin/start-history-server.sh

### serialization
* java 
* kyro
conf.set("spark.serializer", "org.apache.spark.serializer.KryoSerializer")
conf.set("spark.scheduler.allocation.file", "/path/to/allocations.xml")
### instrumentations
* Ganglia
* OS profiling tools
* JVM utilities

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
```

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

### libraries
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
