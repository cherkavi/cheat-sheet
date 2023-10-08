## links
* [java 8 download](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html)
* [java decompiler, class decompile](http://java-decompiler.github.io/)
* [useful links](http://www.programcreek.com/2012/11/top-100-java-developers-blogs/)
* [source code examples](http://www.javased.com/)
* [source code examples](https://searchcode.com)
* [java 8 json](https://habr.com/company/luxoft/blog/280782/)
* [java links](https://github.com/Vedenin/useful-java-links)
* [profiler](https://www.yourkit.com/)
* [byte code transformation](https://bytebuddy.net/#/)
* [docker container creator](https://github.com/GoogleContainerTools/jib)

### heap dump, [jmap](https://docs.oracle.com/en/java/javase/14/docs/specs/man/jmap.html)
```bash
# find process id for target java process
ps auxf

# find amount of threads inside JVM
ps -eww H -p $JAVA_PROCESS_ID

# make heap dump
jmap -histo $JAVA_PROCESS_ID
jmap -clstats $JAVA_PROCESS_ID
jmap -heap $JAVA_PROCESS_ID
jmap -dump:format=b,live,file=$PATH_TO_OUTPUT_HEAP_DUMP $JAVA_PROCESS_ID
```

### java repl java code evaluation
```sh
$JAVA_HOME/bin/jshell -v
```
```java
import java.util.UUID;
UUID.randomUUID();
```

### execute javascript code
```sh
#! /usr/bin/env jjs
print('start of the script');
var variableInScript = 5 * 3;
print(" output from script ${threeyr}");
```

### execute script code
```sh
# inline
jrunscript -e "cat('https://google.com')"
# js repl
jrunscript
```
```
js> t = new java.lang.Thread(function() { print('java Thread in script\n'); })
js> t.start()
```

### JVM parameters
```sh
-Xmx56000m 
-XX:+HeapDumpOnOutOfMemoryError 
-XX:HeapDumpPath==/usr/src/bin/log/ 
-XX:OnOutOfMemoryError=/usr/src/bin/start-app.sh
-XX:ErrorFile=/usr/src/bin/log/fatal-errors.log 
-XX:+PrintGCDetails 
-XX:+UseG1GC 
-Xloggc:/usr/src/bin/log/jvm-gc.log 
```

### JMX java inspect java state java stuck
```bash
# command line argument
-Dcom.sun.management.jmxremote
-Dcom.sun.management.jmxremote.port=5006
-Dcom.sun.management.jmxremote.rmi.port=5006
-Djava.rmi.server.hostname=127.0.0.1 
-Dcom.sun.management.jmxremote.local.only=false
-Dcom.sun.management.jmxremote.authenticate=false
-Dcom.sun.management.jmxremote.ssl=false
# additional parameters
-Djmx.rmi.registry.port=5006 
-Djmx.rmi.port=5006
```
```bash
# OpenShift settings
# connect to openshift
$ oc login $OC_HOST:8443
 
# forward ports from localhost to pod
# oc port-forward $POD_NAME <local port>:<remote port>
$ oc port-forward $POD_NAME 5006
 
# e.g. connect to the jmx port with visual vm
visualvm --openjmx localhost:5006
jconsole localhost:5006
```

### visualvm 
#### [visual vm download](https://visualvm.github.io/download.html)
#### set specific jdk
vim etc/visualvm.conf
```sh
visualvm_jdkhome="/home/my-user/.sdkman/candidates/java/18.0.1-oracle"
```
#### for connecting to jvm - select "new jmx connection"

### java application debug, remote debug
java <line below> -jar <jar file path> ... 
```bash
-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=5005
```
```bash
-agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=localhost:5005
```
The Java image on OpenShift has built-in support for remote debugging
```sh
# environment variables
JAVA_DEBUG=true  
# or
oc set env dc/inventory JAVA_DEBUG=true
```
	
you can create SSH tunnel between your local machine and remote:
( execute next line from local machine )
```bash
ssh -L 7000:127.0.0.1:5005 $REMOTE_USER@$REMOTE_HOST
```
and your connection url will looks like: 127.0.0.1:7000  
or  
connect with local openshift client
```bash
# connect to openshift
$ oc login $OPENSHIFT_HOST:8443

# forward the jmx ports
$ oc port-forward $POD_NAME 5005
```

### java application console debug headless debug
![image](https://user-images.githubusercontent.com/8113355/176193336-cd4c4e30-9752-4fb2-8721-3e43218b3b64.png)

connect to process
```sh
jdb -attach localhost:5005
jdb -attach localhost:5005 -sourcepath :src/main/java/
```
commands inside debugger
```
# set breakpoint on line
stop at com.ubs.ad.data.interval.v2.IntervalServiceImpl:189
# set breakpoint on method
stop at com.ubs.ad.data.interval.v2.IntervalServiceImpl.getAvailability

# print list of breakpoints
clear  

# remove breakpoint 
clear com.ubs.ad.data.interval.v2.IntervalServiceImpl:189

# print local variables 
locals

# for all methods need to use full name of the class
print com.ubs.interval.IntervalValidator.isContributorIdValid(subscriber)
eval com.ubs.interval.IntervalValidator.isContributorIdValid(subscriber)

# print current stack trace, print position
where 

print intervalsIdList
dump intervalsIdList
set intervalsIdList=new ArrayList<>();
```
movements inside debugger
```
next                      -- step one line (step OVER calls)
cont                      -- continue execution from breakpoint

step                      -- execute current line ( step in )
step up                   -- execute until the current method returns to its caller
stepi                     -- execute current instruction
```


### java agent
```bash
-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=9010
```
```sh
java -javaagent:<agent-path.jar>=<agent args> -jar <your-jar.jar>
```

### attach to process, agent to pid, java agent
```java
// Using com.sun.tools.attach.VirtualMachine:
VirtualMachine vm = VirtualMachine.attach(pid)
vm.loadAgent(jarPath, agentArgS)
vm.detach()
```
### get current java version java runtime version
```java
System.getProperty("java.runtime.version");
```

### set proxy system properties
```
System.getProperties().put("http.proxyHost", "someProxyURL");
System.getProperties().put("http.proxyPort", "someProxyPort");

-Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=3128 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=3129
```
```sh
export _JAVA_OPTIONS="-Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=3128 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=3128 -Dhttp.nonProxyHosts=localhost|*.ubsgroup.net|*.zur -Dhttps.nonProxyHosts=localhost|*.ubsgroup.net|*.muc"
```

### print all classes for running application
```sh
java -verbose:class -version
java -verbose:class --classpath:my.jar  ClassWithMain
```

### certificates
#### create certificate 
```
Browser.address url.lock(broken lock) -> Certificate -> Details -> Copy to File -> Base-64 encoded X.509 -> metrics.cer
# keytool -printcert -sslserver path.to.server.com:443
```
#### read certificate 
```sh
openssl x509 -text -noout -in elastic-staging.cer 
```
#### import certificate to truststore 
```sh
ls myTrustStore && rm myTrustStore;
CERT_FILE=metrics.cer
keytool -printcert -rfc -file $CERT_FILE
keytool -printcert -file $CERT_FILE

CERT_ALIAS=metrics
TRUST_STORE=myTrustStore
keytool -import -file $CERT_FILE -alias $CERT_ALIAS -keystore $TRUST_STORE

# list of certificates in truststore 
keytool -list -keystore $TRUST_STORE

# export and print certificate
keytool -exportcert -keystore $TRUST_STORE -alias $CERT_ALIAS -file out.crt
keytool -printcert -file out.crt
```

#### create empty truststore
```sh
TRUST_STORE=emptyStore
ls $TRUST_STORE && rm $TRUST_STORE;
CERT_ALIAS=garbage
STORE_PASS="my_pass"

# create truststore with one record
keytool -genkeypair -alias $CERT_ALIAS -storepass $STORE_PASS -keypass secretPassword -keystore $TRUST_STORE -dname "CN=Developer, OU=Department, O=Company, L=City, ST=State, C=CA"
# list of records
keytool -list -keystore $TRUST_STORE -storepass $STORE_PASS
## delete records
keytool -delete -alias $CERT_ALIAS -storepass $STORE_PASS -keystore $TRUST_STORE
# list of records 
keytool -list -keystore $TRUST_STORE -storepass $STORE_PASS
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

### take a look into jar, list all files into jar
```
jar tvf WC2.jar
```

### print loaded classes during start application
```
java -verbose app
```

### Security
[collaboration with underlying system javacallbacks](https://docs.oracle.com/javase/8/docs/api/javax/security/auth/callback/Callback.html)

### LOG4j
#### log4j configuration key for JVM 
```
-Dlog4j.configuration={path to file}
```

#### log4j override configuration from code
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

#### log4j file configuration 
```
<?xml version="1.0" encoding="UTF-8"?>
    <Configuration status="WARN">
      <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
          <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
      </Appenders>
      <Loggers>
        <Logger name="com.foo.Bar" level="debug">
          <AppenderRef ref="Console"/>
        </Logger>
        <Root level="debug">
          <AppenderRef ref="Console"/>
        </Root>
      </Loggers>
    </Configuration>
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
---
liquibase print sql scripts ( update sql )
```
java -jar C:\soft\maven-repo\org\liquibase\liquibase-core\3.4.1\liquibase-core-3.4.1.jar  \
--driver=oracle.jdbc.OracleDriver --classpath=C:\soft\maven-repo\com\oracle\ojdbc6\11.2.0.2.0\ojdbc6-11.2.0.2.0.jar  \
--changeLogFile=C:\temp\liquibase\brand-server\master.xml  \
--contexts="default" \
--url=jdbc:oracle:thin:@q-ora-db-scan.wirecard.sys:1521/stx11de.wirecard --username=horus_user_cherkavi --password=horus_user_cherkavi \
 updateSQL > script.sql
 ```
---
liquibase test scripts against H2 database
```
java -jar liquibase-core-3.5.5.jar  --driver=org.h2.Driver --classpath=h2-1.4.197.jar --changeLogFile=C:\project\opm\opm-persistence\src\main\resources\db\changelog\changes.xml --url=jdbc:h2:mem:testdb;Mode=Oracle --username=sa --password=  update
```

### flyway
[commandline documentation](https://flywaydb.org/documentation/commandline/)
---
folder structure
```
/-<root>
  /drivers
  /jars
  /scripts
  /sql
```
---
execute flyway from commandline, db info
```
java -cp flyway-core-4.0.3.jar;flyway-commandline-5.1.3.jar org.flywaydb.commandline.Main -configFile=flyway-info.conf info
```
---
to see debug log, need to add next jars into classpath:
* slf4j-api-1.7.25.jar
* log4j-to-slf4j-2.10.0.jar
* log4j-core-2.10.0.jar
* log4j-api-2.10.0.jar
* log4j-1.2.14.jar
---
slf4j logging
```
lib/log4j-core-2.11.0.jar:\
lib/log4j-api-2.11.0.jar:\
lib/log4j-slf4j-impl-2.11.0.jar:\
```
and execute
```sh
java -Dlog4j2.debug=true 
```

config file example
```
flyway.driver=oracle.jdbc.OracleDriver
flyway.url=jdbc:oracle:thin:@vpn050.kabel.de:1523:PMDR
flyway.user=login
flyway.password=pass
flyway.locations=filesystem:../fixnet-data/,classpath:db.migration.initialize
```
---
execute flyway with command line parameters
```
flyway migrate -driver=oracle.jdbc.OracleDriver -url=jdbc:oracle:thin:@vs050:1523:PMDR -user=xnet -password=xnet -locations=filesystem:/opt/oracle/scripts"
```
--- 
flyway table with current migration status
```
select * from {schema name}."schema_version";
```
---
update schema name with "success" flag
```
update {schema name}."schema_version" set "success"=1 where "version"='3.9'; 
```
---
update from java code, custom update from code, 
```java
package db.migration.initialize;
import org.flywaydb.core.api.migration.jdbc.JdbcMigration;
public class V1_9_9__insert_initial_gui_configurations implements JdbcMigration {
    @Override
    public void migrate(Connection connection) throws Exception
}
```
```
<location>classpath:db.migration.initialize</location>
```
---
[flyway crc generation flyway control sum generator](https://github.com/flyway/flyway/blob/main/flyway-core/src/main/java/org/flywaydb/core/internal/resolver/ChecksumCalculator.java#L59)
```java
    public static int calculateSum(String filePath) {

        final CRC32 crc32 = new CRC32();
 
        try (BufferedReader bufferedReader = new BufferedReader(new FileReader(new File(filePath))); ) {
            String line = null;
            while ((line = bufferedReader.readLine()) != null) {
		    line = BomFilter.FilterBomFromString(line);
	            crc32.update(StringUtils.trimLineBreak(line).getBytes(StandardCharsets.UTF_8));
            }
        } catch (IOException e) {
            System.err.printf("Error while trying to calculate CRC for file %s: %s\n", filePath, e.getMessage());
            System.exit(1);
        }
 
        int checksum = (int) crc32.getValue();
        return checksum;
    }
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

## [Test playground](https://github.com/in28minutes/spring-boot-rest-api-playground)

## Sonar
### skip checking by code checker
```
// NOSONAR
```
```
@java.lang.SuppressWarnings("squid:S3655")
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

## BPMN
* [Activiti](https://github.com/cherkavi/java-code-example/tree/master/activiti-example)
* [Flowable](https://www.flowable.com/open-source/docs/)
	
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
# [OpenXava](https://openxava.org/)
```
OpenXava create new project from DataSource
	1 - switch to workspace %OPENXAVA%/workspace
	2 - open project OpenXavaTemplate
	3 - execute ant script: CreateNewProject.xml
	4 - enter name of project (Monolith) 
	5 - import new project into workspace ( import existing project )
	6 - replace dialect - %OPENXAVA%/workspace/Monolith/persistence/hibernate.cfg.xml
	7 - replace dialect - %OPENXAVA%/workspace/Monolith/persistence/META-INF/persistence.xml
	8 - create hibernate mapping from Database ( by JBoss tool )
	8.1 - create Hibernate console
	8.2 - create Hibernate Code Generation Configuration
	8.3 - run Hibernate Code Generation Configuration
	9 - add annotations:
	9.1 - add import to each Domain file: 
		import javax.persistence.*;
		import org.openxava.annotations.*;
	9.2 - add annotations to each column:
 	    @Id
	    // GeneratedValue(strategy=GenerationType.AUTO)
	    @Hidden
	    @Column(name = "value_id", length=36, nullable = false)
	    private String valueId;
	
    	    @Required
    	    @Column(name = "group", length = 20, nullable = false)
	    private String group;
	
	10 - add openxava annotations to each Entity:
	11 - execute ant compile ( %OPENXAVA%/workspace/Monolith/  )
	12 - execute ant deployWar ( %OPENXAVA%/workspace/Monolith/  )
	13 - execute tomcat
	14 - goto 
		http://localhost:8080/Monolith/modules/Value
		<     tomcat   path ><Project >       < entity name >
```

## Version differences
![java 8 to 9](https://i.postimg.cc/MHynycjm/java-8-9.png)

## [jwt web token decoder JSON web token](https://jwt.io/)
