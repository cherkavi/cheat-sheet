## source code links
* [useful links](http://www.programcreek.com/2012/11/top-100-java-developers-blogs/)
* [source code examples](http://www.javased.com/)
* [source code examples](https://searchcode.com)

### @Null
```
use Optional.absent
```

### locking
```
Reentrant lock == Semaphore
```

## jshell
### print import
```
/!
```

### help
```
/help
/?
```

### exit
```
/exit
```

## JAR
### create jar based on compiled files
```
jar cf WC2.jar *.class
```

### take a look into jar
```
jar tf WC2.jar
```

### print loaded classes during start application
```
java -verbose app
```

### log4j configuration key for JVM 
```
-Dlog4j.configuration={path to file}
```

### log4j override configuration from code
```
Properties props = new Properties();
props.put("log4j.rootLogger", level+", stdlog");
props.put("log4j.appender.stdlog", "org.apache.log4j.ConsoleAppender");
props.put("log4j.appender.stdlog.target", "System.out");
props.put("log4j.appender.stdlog.layout", "org.apache.log4j.PatternLayout");
props.put("log4j.appender.stdlog.layout.ConversionPattern","%d{HH:mm:ss} %-5p %-25c{1} :: %m%n");
// Execution logging
props.put("log4j.logger.com.hp.specific", level);
// Everything else 
props.put("log4j.logger.com.hp", level);
LogManager.resetConfiguration();
PropertyConfigurator.configure(props);
```

### log4j update configuration during runtime, refresh configuration
```
<?xml version="1.0" encoding="UTF-8"?>
<Configuration monitorInterval="30">
...
</Configuration>
```

### h2 Oracle dialect
```
spring.datasource.url=jdbc:h2:mem:testdb;Mode=Oracle
spring.datasource.platform=h2
spring.jpa.hibernate.ddl-auto=none
spring.datasource.continue-on-error=true
```

### hsqldb Oracle dialect
```
driverClassName="org.hsqldb.jdbc.JDBCDriver"
url="jdbc:hsqldb:mem:test;sql.syntax_ora=true"
username="sa" password=""
```

### liquibase
####  liquibase print sql scripts ( update sql )
```
java -jar C:\soft\maven-repo\org\liquibase\liquibase-core\3.4.1\liquibase-core-3.4.1.jar  \
--driver=oracle.jdbc.OracleDriver --classpath=C:\soft\maven-repo\com\oracle\ojdbc6\11.2.0.2.0\ojdbc6-11.2.0.2.0.jar  \
--changeLogFile=C:\temp\liquibase\brand-server\master.xml  \
--contexts="default" \
--url=jdbc:oracle:thin:@q-ora-db-scan.wirecard.sys:1521/stx11de.wirecard --username=horus_user_cherkavi --password=horus_user_cherkavi \
 updateSQL > script.sql
 ```


### java application debug
```
-Dcom.sun.management.jmxremote
-Dcom.sun.management.jmxremote.port=9010
-Dcom.sun.management.jmxremote.local.only=false
-Dcom.sun.management.jmxremote.authenticate=false
-Dcom.sun.management.jmxremote.ssl=false
```

### JNDI datasource examples:
```
   <Resource name="ds/JDBCDataSource" auth="Container"
              type="javax.sql.DataSource" 
              driverClassName="org.h2.Driver"
              url="jdbc:h2:~/testdb;Mode=Oracle"
              username="sa" 
              password="" maxActive="20" maxIdle="10"
              maxWait="-1"/>

   <Resource name="ds/JDBCDataSource" auth="Container"
              type="javax.sql.DataSource" 
              driverClassName="org.hsqldb.jdbc.JDBCDriver"
              url="jdbc:hsqldb:mem:test;sql.syntax_ora=true"
              username="sa" 
              password="sa" maxActive="20" maxIdle="10"
              maxWait="-1"/>
```


## WildFly
### check health of application
```
<host:port>/<application>/node
```

## Java Testing
### insert private fields
```
ReflectionUtils.makeAccessible(id);
ReflectionUtils.setField(id, brand, "myNewId");
```

### order of tests execution
```
@FixMethodOrder
```

### rest testing
```
RestAssured
body("brands.size()")
```

## Hibernate
### @Modifying
```
for custom queries only
```

### force joining
```
@WhereJoinTable
```

### inheritance types
```
- table per hierarchy ( SINGLE_TABLE )
- table per sub-class ( JOINED )
- table per class ( TABLE_PER_CLASS )
```

## Spring
### bean post processor
```
@Bean public BeanPostProcessor{return new BeanPostProcessor(){}}
```

### default name of annotated @Bean
```
class name
name of the declared method
```

### to specify the name of @Bean
```
@Qualifier
```

### refresh context for testing
```
@DirtiesContext
```

### for starting context
```
@SpringBootTest instead of @BootstrapWith
```

### using bean by name
```
@Resource(name="${<name of the value>}")
```

### spring boot actuator, spring boot list of all elements
```
http://localhost:8808/actuator
```

### health check of SpringBoot application
```
<url:port>/<application>/health
```

### set system properties
```
System.getProperties().put("http.proxyHost", "someProxyURL");
System.getProperties().put("http.proxyPort", "someProxyPort");
```
