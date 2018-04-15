## Plugins:

### spring boot project 
> mvn spring-boot:run

      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <version>${spring-boot.version}</version>
        <executions>
          <execution>
            <goals>
              <goal>repackage</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <jvmArguments>-Djava.net.preferIPv4Stack=true -Dserver.port=9000 -Dspring.cloud.kubernetes.enabled=false</jvmArguments>
        </configuration>
      </plugin>


### fabric8 with SpringBoot deployment
> mvn fabric8
> mvn fabric8:deploy
> mvn fabric8:undeploy

      <plugin>
        <groupId>io.fabric8</groupId>
        <artifactId>fabric8-maven-plugin</artifactId>
        <version>${fabric8.maven.plugin.version}</version>
        <executions>
          <execution>
            <goals>
              <goal>resource</goal>
              <goal>build</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <resources>
              <labels>
                  <all>
                      <property>
                          <name>app</name>
                          <value>{app-name}</value>
                      </property>
                  </all>
              </labels>
          </resources>
          <enricher>
            <excludes>
              <exclude>spring-boot-health-check</exclude>
            </excludes>
          </enricher>
          <generator>
            <includes>
              <include>spring-boot</include>
            </includes>
            <config>
              <spring-boot>
                <from>registry.access.redhat.com/redhat-openjdk-18/openjdk18-openshift:1.1</from>
              </spring-boot>
            </config>
          </generator>
        </configuration>
      </plugin>


### wildfly project
> mvn wildfly-swarm:run

      <plugin>
        <groupId>org.wildfly.swarm</groupId>
        <artifactId>wildfly-swarm-plugin</artifactId>
        <version>${version.wildfly.swarm}</version>
        <executions>
          <execution>
            <goals>
              <goal>package</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <properties>
            <java.net.preferIPv4Stack>true</java.net.preferIPv4Stack>
          </properties>
          <jvmArguments>-Dswarm.http.port=9001</jvmArguments>
        </configuration>
      </plugin>


### fabric8 with WildFly, openshift with WildFly, WildFly Swarm
> mvn fabric8
> mvn fabric8:deploy
> mvn fabric8:undeploy

      <plugin>
        <groupId>io.fabric8</groupId>
        <artifactId>fabric8-maven-plugin</artifactId>
        <version>${fabric8.maven.plugin.version}</version>
        <executions>
          <execution>
            <goals>
              <goal>resource</goal>
              <goal>build</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <resources>
              <labels>
                  <all>
                      <property>
                          <name>app</name>
                          <value>my-app</value>
                      </property>
                  </all>
              </labels>
          </resources>
          <generator>
            <includes>
              <include>wildfly-swarm</include>
            </includes>
            <config>
              <wildfly-swarm>
                <from>registry.access.redhat.com/redhat-openjdk-18/openjdk18-openshift:1.1</from>
              </wildfly-swarm>
            </config>
          </generator>
          <enricher>
            <excludes>
              <exclude>wildfly-swarm-health-check</exclude>
            </excludes>
          </enricher>
        </configuration>
      </plugin>

### set version of source code 
	<build>
		<plugins>
			<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<version>3.1</version>
			<configuration>
				<source>1.6</source>
				<target>1.6</target>
			</configuration>
			</plugin>
		</plugins>
	</build>

### maven war plugin
      <plugin>
        <artifactId>maven-war-plugin</artifactId>
        <version>3.1.0</version>
        <configuration>
          <failOnMissingWebXml>false</failOnMissingWebXml>
        </configuration>
      </plugin>


### maven exec plugin
> mvn exec:java

    <build>
        <plugins>
          <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.4.0</version>
            <executions>
              <execution>
                <goals>
                  <goal>java</goal>
                </goals>
              </execution>
            </executions>
            <configuration>
              <mainClass>org.cherkashyn.vitalii.test.App</mainClass>
              <arguments>
                <argument>argument1</argument>
              </arguments>
              <systemProperties>
                <systemProperty>
                  <key>myproperty</key>
                  <value>myvalue</value>
                </systemProperty>
              </systemProperties>
            </configuration>
          </plugin>
        </plugins>
      </build>

### copy into package additional resources 


    <resources>
      <resource>
        <directory>src/main/java</directory>
        <includes>
          <include> **/*.java </include>
          <include> **/*.properties </include>
          <include> **/*.xml </include>
        </includes>
      </resource>
      <resource>
        <directory>src/test/java</directory>
        <includes>
          <include> **/*.java </include>
          <include> **/*.properties </include>
          <include> **/*.xml </include>
        </includes>
      </resource>
    </resources>


## create project

### example of creating project 
> mvn archetype:generate -DgroupId=com.cherkashyn.vitalii.startup.searchcorrector -DartifactId=searchcorrector -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

### example of creating project
> mvn archetype:generate -DgroupId=com.cherkashyn.vitalii.startup.searchcorrector -DartifactId=workplace -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false

### maven create Java web project
> mvn archetype:generate -DgroupId=com.cherkashyn.vitalii.smava.onsite -DartifactId=soap-calculator -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false

### for creating Eclipse Web project ( change pom.xml:packaging to "war" ) :
> mvn eclipse:eclipse -Dwtpversion=2.0

### Java Vaadin project
> mvn archetype:generate -DarchetypeGroupId=com.vaadin -DarchetypeArtifactId=vaadin-archetype-application -DarchetypeVersion=7.2.5 -DgroupId=com.cherkashyn.vitalii.tools.barcode.ui -DartifactId=BarCodeUtilsUI -Dversion=1.0 -Dpackaging=war

### Java console application
> mvn archetype:generate -DgroupId=com.cherkashyn.vitalii.akka.web -DartifactId=akka-web -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
> mvn archetype:generate -DgroupId=com.cherkashyn.vitalii.testtask.kaufland -DartifactId=anagrams -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

### Java OSGi bundle
> mvn archetype:generate -DarchetypeGroupId=org.apache.karaf.archetypes -DarchetypeArtifactId=karaf-bundle-archetype -DarchetypeVersion=2.3.5 -DgroupId=com.cherkashyn.vitalii.osgi.test.listener -DartifactId=osgi-service-listener -Dversion=1.0.0-SNAPSHOT

### Java OSGi Blueprint bundle
> mvn archetype:generate -DarchetypeGroupId=org.apache.karaf.archetypes -DarchetypeArtifactId=karaf-blueprint-archetype -DarchetypeVersion=2.3.5 -DgroupId=com.cherkashyn.vitalii.osgi.test -DartifactId=osgi-blueprint-consumer -Dversion=1.0.0-SNAPSHOT

### Java OSGi Karaf bundle
> mvn archetype:generate -DarchetypeGroupId=org.apache.karaf.archetypes -DarchetypeArtifactId=karaf-bundle-archetype -DarchetypeVersion=2.2.8 -DgroupId=com.mycompany -DartifactId=KarafExample -Dversion=1.0-SNAPSHOT -Dpackage=com.mycompany.bundle

## Tools:
### how to debug
> %MAVEN_HOME%/bin/mvnDebug

### Download Sources and JavaDoc
> -DdownloadSources=true -DdownloadJavadocs=true
