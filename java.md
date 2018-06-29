## source code links
* [useful links](http://www.programcreek.com/2012/11/top-100-java-developers-blogs/)
* [source code examples](http://www.javased.com/)
* [source code examples](https://searchcode.com)
* [java 8 json](https://habr.com/company/luxoft/blog/280782/)
* [java links](https://github.com/Vedenin/useful-java-links)

### set proxy system properties
```
System.getProperties().put("http.proxyHost", "someProxyURL");
System.getProperties().put("http.proxyPort", "someProxyPort");

-Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=3128 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=3129
```

### execute application from java, execute sub-process, start program
#### start and waiting for finish
```
new ProcessExecutor().commandSplit(executeLine).execute();
```

#### start in separate process without waiting
```
new ProcessExecutor().commandSplit(executeLine).start();
```

#### with specific directory
```
new ProcessExecutor().commandSplit(executeLine).directory(executableFile.getParentFile()).start();
```

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

### liquibase test scripts against H2 database
```
java -jar liquibase-core-3.5.5.jar  --driver=org.h2.Driver --classpath=h2-1.4.197.jar --changeLogFile=C:\project\opm\opm-persistence\src\main\resources\db\changelog\changes.xml --url=jdbc:h2:mem:testdb;Mode=Oracle --username=sa --password=  update
```

### java application debug, remote debug
```
-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=9010
```
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

## Vaadin
### [custom components](https://vaadin.com/directory)
### [spring integration](https://docs.camunda.org/manual/7.4/user-guide/spring-framework-integration/configuration/)
### [horizontal stepper, human progress line, to-do list](http://mekaso.rocks/material-vaadin)

### show frame 
```
    void showFrame(File file){
        Window window = new Window();
        window.setWidth("90%");
        window.setHeight("90%");
        BrowserFrame e = new BrowserFrame("PDF File", new FileResource(file));
        e.setWidth("100%");
        e.setHeight("100%");
        window.setContent(e);
        window.center();
        window.setModal(true);
        UI.getCurrent().addWindow(window);
    }
```

### debug window
```
http://localhost:8090/?debug
```

## Derby
* [tutorial](https://db.apache.org/derby/papers/DerbyTut/ns_intro.html#start_ns)
* [admin page](http://db.apache.org/derby/docs/10.10/adminguide/tadmincbdjhhfd.html)

into 'bin' folder add two variables for all scripts:
```
 export DERBY_INSTALL=/dev/shm/db/db-derby-10.14.2.0-bin/ 
 export DERBY_HOME=/dev/shm/db/db-derby-10.14.2.0-bin/ 
```

start with listening all incoming request ( not from localhost )
```
./startNetworkServer -h vldn338
```
create DB before using, from jdbc url
```
jdbc:derby://vldn338:1527/testDB;create=true
```

maven dependency
```
<dependency>
    <groupId>org.apache.derby</groupId>
    <artifactId>derbyclient</artifactId>
    <version>10.14.2.0</version>
</dependency>
```

jdbc
```
Driver: org.apache.derby.jdbc.ClientDriver
jdbc: jdbc:derby://vldn338:1527/dbName
user: <empty>
pass: <empty>
```

## Activiti
### [user guide](https://www.activiti.org/userguide)
### [eclipse plugin](http://www.activiti.org/designer/update)
### [additional documentation](https://docs.camunda.org/manual/7.5/user-guide/process-engine/process-engine-concepts/)

### create/init DB
```
activiti-engine-x.x.x.jar/org/activiti/db/create/activiti.create.sql
```

## email, e-mail, smtp emulator
[FakeSMTP](http://nilhcem.com/FakeSMTP/)
```
java -jar fakeSMTP.jar --help
```
run example:
```
java -jar fakeSMTP.jar -s -b -p 2525 -a 127.0.0.1  -o output_directory_name 
```

## e-mail server smtp, pop3, imap with SSL
[MailServer](http://www.icegreen.com/greenmail/)
```
java -Dgreenmail.smtp.hostname=0.0.0.0 -Dgreenmail.smtp.port=2525 -Dgreenmail.pop3.hostname=0.0.0.0 -Dgreenmail.pop3.port=8443 -Dgreenmail.users=Vitali.Cherkashyn:vitali@localhost.com,user:password@localhost.com -jar  greenmail-standalone-1.5.7.jar 
```

## WebLogic REST management
[using REST services](http://www.oracle.com/webfolder/technetwork/tutorials/obe/fmw/wls/12c/manage_wls_rest/obe.html)

---
list of all DataSources:
```
curl -X GET http://host:7001/management/wls/latest/datasources
```

---
return all accessible parameters for request
```
curl -X OPTION http://host:7001/management/wls/latest/datasources
```

---
request with authentication ( where magic string is "username:password" in base64 )
```
curl -H "Authorization: Basic d2VibG9naWM6d2VibG9naWMx" -X GET http://host:7001/management/wls/latest/datasources
```

---
request with authentication 
```
curl --user username:password -X GET http://host:7001/management/wls/latest/datasources
```

--- 
description of one DataSource:
```
curl -X GET http://host:7001/management/wls/latest/datasources/id/{datasourceid}
```

---
domain runtime
```
curl --user weblogic:weblogic1  -X GET http://host:7001/management/weblogic/latest/domainRuntime
```

---
server parameters, server folders, server directories, server status, server classpath
```
curl --user weblogic:weblogic1  -X GET http://host:7001/management/weblogic/latest/domainRuntime/serverRuntimes
```

---
list of deployments
```
curl -v --user weblogic:weblogic1 -H X-Requested-By:MyClient -H Accept:application/json -X GET http://host:7001/management/wls/latest/deployments
```

---
list of applications
```
curl --user weblogic:weblogic1 -H Accept:application/json -X GET http://host:7001/management/wls/latest/deployments/application
```

---
info about one application
```
curl --user weblogic:weblogic1 -H Accept:application/json -X GET http://host:7001/management/wls/latest/deployments/application/id/userappname-gui-2018.02.00.00-SPRINT-7
```

---
remove application, undeploy application, uninstall application
```
curl --user weblogic:weblogic1 -H X-Requested-By:weblogic -H Accept:application/json -X DELETE http://host:7001/management/wls/latest/deployments/application/id/userappname-gui-2018.02.00.00-SPRINT-7
```

---
deploy application
```
curl -X POST --user weblogic:weblogic1 -H X-Requested-By:weblogic -H Accept:application/json -H Content-Type:application/json -d@deploy.json http://host:7001/management/wls/latest/deployments/application
```
```
{
    "name":"userappname-gui-2018.02.00.00-SPRINT-7"
   ,"deploymentPath":"/opt/oracle/domains/pportal_dev/binaries/userappname-gui-2018.02.00.00-SPRINT-7.war"
   , "targets": ["pportal_group"]
}
```
