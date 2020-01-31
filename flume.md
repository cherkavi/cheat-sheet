# Architecture
[documentation](https://flume.apache.org/FlumeUserGuide.html)
contains from three tier:
* Agent tier has Flume agent installed
agent is sending data to Collect tier
* Collector tier 
aggregate the data push data to Storage tier
* Storage tier

Each tier has
* Source
* Sink
* Channel between them

Source and Sink used Avro
( Remote procedure call and serialization framework )

Interceptors can be configured for simple data processing

---
## example of the configuration 
```
#
# bin/flume-ng agent -name agent_1 -c conf -f conf/flume-conf-file-log.properties -Dflume.root.logger=INFO,console
#

# aliases for layers
agent_1.sources = folderSource
agent_1.channels = memoryChannel
agent_1.sinks = loggerSink


# layers description
agent_1.sources.folderSource.type = spooldir
agent_1.sources.folderSource.poolDelay = 500 
agent_1.sources.folderSource.spoolDir = /home/technik/temp/flume-example/source
agent_1.sources.folderSource.fileHeader = true
agent_1.sources.folderSource.batchSize=50
agent_1.sources.folderSource.channels = memoryChannel
#          ^   ^
#         |     |
#        |       |
#       | channel |
#        |       |
#         |     |
#          V   V
agent_1.sinks.loggerSink.channel = memoryChannel
agent_1.sinks.loggerSink.type = logger
agent_1.sinks.loggerSink.batch-size=50

# description of channel between Source and Sink
agent_1.channels.memoryChannel.type = memory
agent_1.channels.memoryChannel.capacity = 100



# ---------------------------------------------
# aliases for layers
agent_2.sources = pythonScriptExample
agent_2.channels = memoryChannel
agent_2.sinks = loggerSink

#import time
#if __name__=="__main__":
#	counter = 0
#	# for _ in range (10):
#	while True:
#		time.sleep(0.1)
#		print("next value is: "+str(counter))
#		counter = counter + 1

agent_2.sources.pythonScriptExample.type = exec
agent_2.sources.pythonScriptExample.batchSize = 1
agent_2.sources.pythonScriptExample.batchTimeout = 1
agent_2.sources.pythonScriptExample.command = python /home/technik/temp/flume-example/seq-gen.py
agent_2.sources.pythonScriptExample.channels = memoryChannel
#          ^   ^
#         |     |
#        |       |
#       | channel |
#        |       |
#         |     |
#          V   V
agent_2.sink s.loggerSink.channel = memoryChannel
agent_2.sinks.loggerSink.type = logger
agent_2.sinks.loggerSink.batch-size=500

# description of channel between Source and Sink
agent_2.channels.memoryChannel.type = memory
agent_2.channels.memoryChannel.capacity = 100
```

---
## interceptor example
```
agent_2.interceptors = myCustomInterceptor
# full path to class, compiled jar should be placed into ./lib
agent_2.interceptors.type=MyInterceptor

```
import java.util.List;

import org.apache.flume.Context;
import org.apache.flume.Event;
import org.apache.flume.interceptor.Interceptor;

public class MyInterceptor implements Interceptor {

    @Override
    public void close()
    {

    }

    @Override
    public void initialize()
    {

    }

    @Override
    public Event intercept(Event event)
    {
        byte[] eventBody = event.getBody();
        System.out.println("next event >>> "+eventBody);
        // event.setBody(modifiedEvent);
        return event;
    }

    @Override
    public List<Event> intercept(List<Event> events)
    {
        for (Event event : events){

            intercept(event);
        }

        return events;
    }

    public static class Builder implements Interceptor.Builder
    {
        @Override
        public void configure(Context context) {
        }

        @Override
        public Interceptor build() {
            return new MyInterceptor();
        }
    }
}
```

---
## change JVM properties, remove debug example
change JAVA_OPTS variable into file (line:225)
bin/flume-ng
```
JAVA_OPTS="-Dcom.sun.management.jmxremote
-Dcom.sun.management.jmxremote.port=4159
-Dcom.sun.management.jmxremote.authenticate=false
-Dcom.sun.management.jmxremote.ssl=false"
```