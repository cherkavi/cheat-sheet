### set proxy system properties
```
System.getProperties().put("http.proxyHost", "someProxyURL");
System.getProperties().put("http.proxyPort", "someProxyPort");
```

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

### logging level, loglevel, springboot logging
```
java -jar myapp.jar --debug
```

## Monitoring
### [Crash](http://www.crashub.org/1.3/reference.html)
```
crash.sh {Process ID}
```
```
		<dependency>
			<groupId>org.crashub</groupId>
			<artifactId>crash.embed.spring</artifactId>
			<version>1.3.2</version>
		</dependency>
		<dependency>
			<groupId>org.crashub</groupId>
			<artifactId>crash.connectors.ssh</artifactId>
			<version>1.3.2</version>
		</dependency>
```
```
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-remote-shell</artifactId>
		</dependency>
```
### spring-shell
#### maven dependency
```
        <dependency>
            <groupId>org.springframework.shell</groupId>
            <artifactId>spring-shell-starter</artifactId>
            <version>2.0.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.shell</groupId>
            <artifactId>spring-shell</artifactId>
            <!-- <version>2.0.0.RELEASE</version> -->
        </dependency>
```
At leas one ShellComponent MUST present !!!
```
import org.springframework.shell.standard.ShellComponent;
import org.springframework.shell.standard.ShellMethod;

@ShellComponent
public class HelloClass {
    @ShellMethod("hello")
    public String hello(){
        return "jello";
    }
}

```
#### skip commands
```
  public static void main(String[] args) throws Exception {
                String[] disabledCommands = {"--spring.shell.command.help.enabled=false"}; 
                String[] fullArgs = StringUtils.concatenateStringArrays(args, disabledCommands);
                SpringApplication.run(MyApp.class, fullArgs);
        }
```
#### skip command exit, skip close sprintshell
```
String[] disabledCommands = {"--spring.shell.command.quit.enabled=false"};
```
#### disable interactive collaboration, disable spring shell
```
spring.shell.interactive.enabled=false
```

### [h2 cheat sheet](http://www.h2database.com/html/cheatSheet.html)

### h2 Oracle dialect
```
spring.datasource.url=jdbc:h2:mem:testdb;Mode=Oracle
spring.datasource.platform=h2
spring.datasource.driver-class-name=org.h2.Driver
spring.jpa.hibernate.ddl-auto=none
spring.datasource.continue-on-error=true
```

### h2 yaml version
```
###
#   Database Settings
###
spring:
  datasource:
    # url: jdbc:h2:mem:user-app;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
    url: jdbc:h2:~/user-manager.h2;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE
    platform: h2
    username: sa
    password:
    driverClassName: org.h2.Driver
  jpa:
    database-platform: org.hibernate.dialect.H2Dialect
    hibernate:
      ddl-auto: update
    properties:
      hibernate:
        show_sql: false
        use_sql_comments: true
        format_sql: true

###
#   H2 Settings
###
  h2:
    console:
      enabled: true
      path: /console
      settings:
        trace: false
        web-allow-others: false
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
### [application.properties](https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html)
### [datasource settings](https://docs.spring.io/spring-boot/docs/current/reference/html/howto-database-initialization.html)
example of initialization during the start app
properties:
```
spring.datasource.initialization-mode = h2
spring.datasource.platform = never
```
files: 
```
schema.sql
schema-${platform}.sql 
```

### command line argument to specify external file with configuration
```
-Dspring.config.location=your/config/dir/application.properties
```

### spring boot another http port, change http port, change server port
```
mvn spring-boot:run -Dserver.port=8090
```

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

### using bean by name
```
@Resource(name="${<name of the value>}")
```

### refresh context for testing
```
@DirtiesContext
```

### for starting context
```
@SpringBootTest instead of @BootstrapWith
```

### spring boot actuator, spring boot list of all elements
```
http://localhost:8808/actuator
```

### health check of SpringBoot application
```
<url:port>/<application>/health
```

### logging level
```
logging:
  level:
    ROOT: DEBUG
```
### springboot h2, h2 console, spring-boot h2
```
import org.h2.server.web.WebServlet;

    @Bean
    @Conditional(OpmGuiConfiguration.H2Contidion.class)
    ServletRegistrationBean h2servletRegistration(){
        ServletRegistrationBean registrationBean = new ServletRegistrationBean( new WebServlet());
        registrationBean.addUrlMappings("/h2-console/*");
        return registrationBean;
    }

    public static class H2Contidion implements Condition{
        @Override
        public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
            return Arrays.asList(conditionContext.getEnvironment().getActiveProfiles()).contains("h2");
        }
    }
```
```
<!-- https://mvnrepository.com/artifact/com.h2database/h2 -->
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <version>1.4.197</version>
    <scope>test</scope>
</dependency>
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

## Activiti
### [user guide](https://www.activiti.org/userguide)
### [eclipse plugin](http://www.activiti.org/designer/update)
### [additional documentation](https://docs.camunda.org/manual/7.5/user-guide/process-engine/process-engine-concepts/)

### create/init DB
```
activiti-engine-x.x.x.jar/org/activiti/db/create/activiti.create.sql
```
