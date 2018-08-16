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
```
logging:
  level:
    ROOT: DEBUG
```
```
-Dlogging.level.root=debug
```

### logging to file, log to file
```
-Dlogging.file 	
-Dlogging.path
```
```
-Dlogging.file=deployer.log -Dlogging.path=/dev/deployer/deployer.log -Dlogging.level.root=info
```

### spring boot another http port, change http port, change server port
```
mvn spring-boot:run -Dserver.port=8090
```

### spring boot start application with specific profile
```
java -Dspring.profiles.active={name of profile} -jar {path to jar/war with spring-boot inside} 
```

### spring boot update DB schema, database update, hibernate update
```
-Dspring.jpa.hibernate.ddl-auto=update
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

### add bean programmatically, add bean at runtime
```
GenericApplicationContext context = ....;
context.registerBean("int100", Integer.class, () -> new Integer(100));
context.registerBean("int100Lazy", Integer.class, () -> new Integer(100), (bd) -> bd.setLazyInit(true));
```

### hierarchy
if @Component extends another @Component, parent won't visible for Spring context

### refresh context for testing
```
@DirtiesContext
```

### for starting context
```
@SpringBootTest instead of @BootstrapWith
```

### spring boot actuator, spring boot list of all elements
dependency
```
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
      </dependency>
```
[endpoints](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-endpoints.html)
```
http://localhost:8808/actuator
```

### health check of Spring Boot application
```
<url:port>/<application>/health
```

### Spring Boot h2, h2 console, Spring Boot h2, conditional bean, register bean if condition is matches
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

### conditional bean, bean with specific methods 
```
    @Bean(initMethod = "start", destroyMethod = "stop")
    @ConditionalOnMissingBean(InstanceRepository.class)
```
### junit test abstract file with custom copmonent scanning
```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(loader = AnnotationConfigContextLoader.class, classes={SpringStepTest.TestConfiguration.class})
public abstract class SpringStepTest {

    @Autowired
    ApplicationContext context;
 
    @Configurable
    @ComponentScan(basePackages = {"com.cd.deployer.step"})
    public static class TestConfiguration {}
}
```

### not to start web application
```
new SpringApplicationBuilder(CommandExecutorApplication.class).web(WebApplicationType.NONE).run(args);
```

### set variable from expression
```
    @Autowired
    JSONReader jsonReader;

    @Value("${steps}")
    String pathToSteps;
    @Value("${failSteps}")
    String pathToFailSteps;

    // Value("#{JSONReader.parse(JSONReader.readFile('${steps}'))}")
    List<StepWithParameters> steps;

    // Value("#{JSONReader.parse(JSONReader.readFile('${failSteps}'))}")
    List<StepWithParameters> failSteps;

    @PostConstruct
    public void afterBuild(){
        this.steps = jsonReader.parse(jsonReader.readFile(Objects.isNull(pathToSteps)?System.getProperty("steps"):pathToSteps));
        this.failSteps = jsonReader.parse(jsonReader.readFile(Objects.isNull(pathToFailSteps)?System.getProperty("failSteps"):pathToFailSteps));
    }

```

## Spring boot issues

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
#### No serialization found for class
```
HttpEntity<String> request = new HttpEntity<>(new ObjectMapper().writeValueAsString(registration), headers);
    or
ObjectMapper mapper = new ObjectMapper();
mapper.setVisibility(PropertyAccessor.FIELD, JsonAutoDetect.Visibility.ANY);
HttpEntity<String> request = new HttpEntity<>(mapper.writeValueAsString(registration), headers);
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
# spring boot maven plugin for building fat jar, uber jar, jar with all dependencies
```
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
```

# spring boot admin
[source code](https://github.com/codecentric/spring-boot-admin)
[doc](http://codecentric.github.io/spring-boot-admin/2.0.0/)
### maven dependency
```
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.2.RELEASE</version>
    <relativePath/> 
  </parent>
  
  <dependencies>
    <dependency>
        <groupId>de.codecentric</groupId>
        <artifactId>spring-boot-admin-starter-server</artifactId>
        <version>2.0.0</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
  </dependencies>
```

### register client on server side
```
    @Bean
    CommandLineRunner registerClient(InstanceRegistry registry){
        return new CommandLineRunner() {
            @Override
            public void run(String... args) throws Exception {
                Registration build = Registration.builder()
                        .name("sit")
                        // .managementUrl("http://v337:9001/env")
                        .healthUrl(    "http://v337:9001/health")
                        .serviceUrl(   "http://v337:9001")
                        // .source(       "http://v337:9001")
                        .build();

                Mono<InstanceId> response = registry.register(build);
                System.out.println(response.block());
            }
        };
    }
```

### curl request to register new instance
```
curl -X POST -H "content-type:application/json;charset=UTF-8" -d "@register-host.json" http://localhost:8080/instances
```
```
{
	"name":"new host",
	"healthUrl": "http://v337:9001/health",
	"serviceUrl": "http://v337:9001",
	"metadata": {
		"version":"1"
		"controlUrl":"my-own-manager:8080"
	}
}
```
### curl delete instance
```
curl -X DELETE -H "content-type:application/json;charset=UTF-8" http://localhost:8080/instances/bbe42fdc3e6a
```
