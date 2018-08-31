### [h2 cheat sheet](http://www.h2database.com/html/cheatSheet.html)

### h2 Oracle dialect
```
spring.datasource.url=jdbc:h2:mem:testdb;Mode=Oracle
spring.datasource.platform=h2
spring.datasource.driver-class-name=org.h2.Driver
spring.jpa.hibernate.ddl-auto=none
spring.datasource.continue-on-error=true
```

### activate console via property file
```
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
spring.h2.console.settings.web-allow-others=true
spring.h2.console.settings.trace=true
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
### print help
```
java -cp h2-1.4.192.jar org.h2.tools.Server -?
```

### Server mode start, web console start
```
java -jar h2-1.4.192.jar  -webAllowOthers -webPort 9108 -tcpAllowOthers -tcpPort 9101 -tcpPassword sa
```
connection
!!! file placed into the same folder when application was started
!!! path with current folder - mandatory
```
jdbc:h2:tcp://localhost:9101/./user-manager.h2
```

### Server mode start, web console start, with specifying folder where file with data placed
```
java -jar h2-1.4.192.jar  -webAllowOthers -webPort 9108 -tcpAllowOthers -tcpPort 9101 -tcpPassword sa -baseDir C:\project\h2-server 
```
connection to server
```
jdbc:h2:tcp://localhost:9101/user-manager.h2
```
connection to server with database creation
```
jdbc:h2:tcp://localhost:9101/new-database-
```


### Server mode connection 
!!! file placed into the same folder when application was started
!!! path with current folder - mandatory
```
jdbc:h2:tcp://localhost:9101/./user-manager.h2
```


### maven dependency
```
    <dependency>
      <groupId>com.h2database</groupId>
      <artifactId>h2</artifactId>
      <version>1.4.197</version>
    </dependency>
```
### create connection
```
    private Connection obtainJdbcConnection(String url, String user, String password) {
        try{
            return DriverManager.getConnection(url, user, password);
        }catch(SQLException ex){
            throw new IllegalArgumentException(String.format("can't obtain connection from jdbc: %s, user:%s, password: %s ", url, user, password), ex);
        }
    }

```
### create user
```
create user if not exists sonar password 'sonar' admin;
```

create schema
```
create schema sonar authorization sonar;
SET SCHEMA sonar;
```
