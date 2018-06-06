## Spring
### [examples](https://github.com/spring-guides/)
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

### logging level, loglevel, spring boot logging
```
java -jar myapp.jar --debug
```


### spring boot another http port, change http port, change server port
```
mvn spring-boot:run -Dserver.port=8090
```

### spring boot start application with specific profile
```
java -Dspring.profiles.active={name of profile} -jar {path to jar/war with spring-boot inside} 
```

### bean post processor
```
@Bean public BeanPostProcessor{return new BeanPostProcessor(){}}
```

### application event listener
[accessible events](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-application-events-and-listeners)
```
    @EventListener
    public void applicationPidFileWriter(ApplicationPreparedEvent event){
        ApplicationPreparedEvent surrogateEvent = new ApplicationPreparedEvent(event.getSpringApplication(), new String[]{}, event.getApplicationContext());
        new ApplicationPidFileWriter().onApplicationEvent(surrogateEvent);
    }
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
[endpoints](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-endpoints.html)
```
http://localhost:8808/actuator
```

### health check of Spring Boot application
```
<url:port>/<application>/health
```

### logging level
```
logging:
  level:
    ROOT: DEBUG
```
```
-Dlogging.level.root=debug
```
### Spring Boot h2, h2 console, Spring Boot h2
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

### Spring boot issues

#### error during start standalone web application
```
Unable to start web server; nested exception is org.springframework.context.ApplicationContextException: 
Unable to start ServletWebServerApplicationContext due to missing ServletWebServerFactory bean.
```
just update pom.xml
```
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
```

#### multipart upload
```
"status":400,"error":"Bad Request","message":"Required request part 'file' is not present","path":"/"
```
application.properties
```
spring.servlet.multipart.max-file-size=128MB
spring.servlet.multipart.max-request-size=128MB
spring.servlet.multipart.enabled=true
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


