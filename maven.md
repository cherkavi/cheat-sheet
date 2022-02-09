## maven scope explanations
![maven scopes](https://user-images.githubusercontent.com/8113355/143955154-eab903a5-a069-4773-a217-e0472c0c621f.png)


## create project, init project, new project

### example of creating project
```
mvn archetype:generate -DgroupId=com.cherkashyn.vitalii.startup.searchcorrector -DartifactId=searchcorrector -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

### example of creating project
``` 
mvn archetype:generate -DgroupId=com.cherkashyn.vitalii.startup.searchcorrector -DartifactId=workplace -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
```

### maven create Java web project
``` 
mvn archetype:generate -DgroupId=com.cherkashyn.vitalii.smava.onsite -DartifactId=soap-calculator -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
```

### for creating Eclipse Web project ( change pom.xml:packaging to "war" ) :
``` 
mvn eclipse:eclipse -Dwtpversion=2.0
```

### build with many threads
```
mvn -T 1C clean install # 1 per thread 
mvn -T 4 clean install # 4 threads
```

### build sub-modules with parent-dependencies
```
mvn -am ...
mvn --also-make ...

```

### list of all modules
```
mvn org.qunix:structure-maven-plugin:modules
```


### buld only one module, single module build
```sh
# mvn --projects common/common-utils clean install
mvn -pl common/common-utils clean install
```
or build with all dependencies
```sh
mvn --threads 2C --projects common/common-utils -am clean install
```
or build 
```sh
mvn clean package -pl '!:processing-common,!:processing-mapr-ojai-common'
```

### dry run 
```
mvn -q -Dexec.executable=echo -Dexec.args='${project.version}' --non-recursive org.codehaus.mojo:exec-maven-plugin:1.3.1:exec
```

### for complex project print dependency tree
```
mvn dependency:tree | grep "^\\[INFO\\] [+\]" | awk '{print $NF}' | grep "^com.cherkashyn.vitalii" | awk -F ':' '{print $1":"$2}' > /home/projects/components.list
```

```
find . -name src | awk -F 'src' '{print $1}' | while read line; do
    echo ">>> " $line "   " `mvn -f $line"pom.xml" dependency:tree | grep "\\[INFO\\] --- maven-dependency-plugin" | awk -F '@' '{print $2}' | awk '{print $1}'`
    mvn  -f $line"pom.xml" dependency:tree | grep "^\\[INFO\\] [+-|\]" | awk '{print $NF}' | grep "^com.cherkashyn.vitalii" | awk -F ':' '{print $1":"$2}' | python python-utilities/console/string-in-list.py /home/projects/components.list
done
```

### build only specific subproject
```
mvn clean install --projects :artifact_id
```

### build test coverage
plugin:  
```
org.scoverage:scoverage-maven-plugin:1.3.0:report
```
```
mvn scoverage:report --projects :artifact_id
```

### single test running, start one test, certain test
https://maven.apache.org/surefire/maven-surefire-plugin/examples/single-test.html  
```bash
# scala
mvn clean -Dsuites=*SpeedLimitSignalsSpec* test
mvn -Dtest=DownloadServiceImplTest* test
```
scalatest
```sh
mvn -Denable-scapegoat-report -Dintegration.skipTests -Dscoverage.skip -Djacoco.skip -Dsuites="*LabelerJobArgumentsTest" test 
```

### cobertura help, 
```
mvn cobertura:help -Ddetail=true
```

### cobertura html
```
mvn cobertura:clean cobertura:cobertura -Dcobertura.report.format=html
```

### Java Vaadin project
``` 
mvn archetype:generate -DarchetypeGroupId=com.vaadin -DarchetypeArtifactId=vaadin-archetype-application -DarchetypeVersion=7.2.5 -DgroupId=com.cherkashyn.vitalii.tools.barcode.ui -DartifactId=BarCodeUtilsUI -Dversion=1.0 -Dpackaging=war
```

### Java console application
``` 
mvn archetype:generate -DgroupId=com.cherkashyn.vitalii.akka.web -DartifactId=akka-web -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
 mvn archetype:generate -DgroupId=com.cherkashyn.vitalii.testtask.kaufland -DartifactId=anagrams -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
 ```

### Java OSGi bundle
``` 
mvn archetype:generate -DarchetypeGroupId=org.apache.karaf.archetypes -DarchetypeArtifactId=karaf-bundle-archetype -DarchetypeVersion=2.3.5 -DgroupId=com.cherkashyn.vitalii.osgi.test.listener -DartifactId=osgi-service-listener -Dversion=1.0.0-SNAPSHOT
```

### Java OSGi Blueprint bundle
``` 
mvn archetype:generate -DarchetypeGroupId=org.apache.karaf.archetypes -DarchetypeArtifactId=karaf-blueprint-archetype -DarchetypeVersion=2.3.5 -DgroupId=com.cherkashyn.vitalii.osgi.test -DartifactId=osgi-blueprint-consumer -Dversion=1.0.0-SNAPSHOT
```

### Java OSGi Karaf bundle
``` 
mvn archetype:generate -DarchetypeGroupId=org.apache.karaf.archetypes -DarchetypeArtifactId=karaf-bundle-archetype -DarchetypeVersion=2.2.8 -DgroupId=com.mycompany -DartifactId=KarafExample -Dversion=1.0-SNAPSHOT -Dpackage=com.mycompany.bundle
```

### debug from IDE, IDE debug
```-DforkCount=0 -DreuseForks=false -DforkMode=never ```

### remote debug, remote projecess debug
``` 
%MAVEN_HOME%/bin/mvnDebug
```

### Download Sources and JavaDoc
``` 
-DdownloadSources=true -DdownloadJavadocs=true
```

### download single artifact, download jar
```
mvn -DgroupId=com.oracle -DartifactId=ojdbc14 -Dversion=10.2.0.4.0 dependency:get
```

### exclude sub-library from dependency lib
            <dependency>
                <groupId>org.quartz-scheduler</groupId>
                <artifactId>quartz</artifactId>
                <version>2.3.0</version>
                <exclusions>
                    <exclusion>
                        <groupId>com.zaxxer</groupId>
                        <artifactId>HikariCP-java6</artifactId>
                    </exclusion>
                </exclusions>
            </dependency>


### describe plugin 
```
mvn help:describe -Dplugin=org.apache.tomcat.maven:tomcat7-maven-plugin
mvn help:describe  -DgroupId=org.springframework.boot -DartifactId=spring-boot-maven-plugin -Ddetail=true
```

Oracle depencdencies
```
            <dependencies>
                <dependency>
                    <groupId>com.github.noraui</groupId>
                    <artifactId>ojdbc7</artifactId>
                    <version>${oracle.driver.version}</version>
                </dependency>
            </dependencies>
```
Oracle driver
```
Class<?> driverClass = Class.forName("oracle.jdbc.driver.OracleDriver");
```

## settings
### location of settings file, maven config
```sh
mvn -X help | grep settings.xml
mvn -X | grep settings.xml
```

### proxy settings
* $MAVEN_HOME/conf/settings.xml
* ${user.home}/.m2/settings.xml
```
<proxies>
    <proxy>
      <active>true</active>
      <protocol>http</protocol>
      <host>proxy.somewhere.com</host>
      <port>8080</port>
      <username>proxyuser</username>
      <password>somepassword</password>
      <nonProxyHosts>www.google.com|*.somewhere.com</nonProxyHosts>
    </proxy>
  </proxies>
```
!!! important - if your password contains $ sign - use \$
or
```sh
mvn compile -Dhttp.proxyHost=10.10.0.100 -Dhttp.proxyPort=8080 -Dhttp.nonProxyHosts=localhost|127.0.0.1 -Dhttp.proxyUser=baeldung -Dhttp.proxyPassword=changeme
```

## Plugins:

### release plugin
```
mvn -f ../pom.xml versions:set -DnewVersion=%1
mvn -f ../pom.xml -N versions:update-child-modules
```

### javadoc
```
mvn javadoc:javadoc
```
```
 -Dmaven.javadoc.skip=true
 ```

### jar without class, no class files
```sh
# put java files to proper place
mkdir src/main/java
```

### uber jar plugin, fat jar, jar with all dependencies, shade plugin
```
<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-assembly-plugin</artifactId>
				<version>2.5.4</version>
				<configuration>
					<descriptorRefs>
						<descriptorRef>jar-with-dependencies</descriptorRef>
					</descriptorRefs>
					<archive>
						<!-- manifestFile>${project.basedir}/src/main/resources/META-INF/MANIFEST.MF</manifestFile -->
            					<manifest>
              						<mainClass>com.cherkashyn.vitalii.tools.App</mainClass>
            					</manifest>
					</archive>
					<!-- Remove the "-jar-with-dependencies" at the end of the file -->
					<appendAssemblyId>false</appendAssemblyId>
				</configuration>
				<executions>
					<execution>
						<goals>
							<goal>attached</goal>
						</goals>
						<phase>package</phase>
					</execution>
				</executions>
			</plugin>
```
```
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>8</source>
                    <target>8</target>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.1.1</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>com.vodafone.SshClient</mainClass>
                                </transformer>
                            </transformers>
                            <shadedArtifactAttached>true</shadedArtifactAttached>
                            <shadedClassifierName>launcher</shadedClassifierName>
                            <!-- <minimizeJar>true</minimizeJar> -->
                            <artifactSet>
                                <excludes>
                                    <exclude>junit:junit</exclude>
                                    <exclude>jmock:*</exclude>
                                    <exclude>*:xml-apis</exclude>
                                    <exclude>org.apache.maven:lib:tests</exclude>
                                </excludes>
                            </artifactSet>
                        </configuration>
                    </execution>
                </executions>
            </plugin>

```

### jgitflow plugin
[official documentation](https://bitbucket.org/atlassian/jgit-flow/wiki/Home)
[configuration description](https://bitbucket.org/atlassian/jgit-flow/wiki/goals.wiki#!common-parameters)
```
                <plugin>
                    <groupId>external.atlassian.jgitflow</groupId>
                    <artifactId>jgitflow-maven-plugin</artifactId>
                    <version>1.0-m5.1</version>
                    <configuration>
                        <enableSshAgent>true</enableSshAgent>
                        <autoVersionSubmodules>true</autoVersionSubmodules>
                        <allowSnapshots>true</allowSnapshots>
                        <releaseBranchVersionSuffix>RC</releaseBranchVersionSuffix>
			<developBranchName>wave3_1.1</developBranchName>
                        <pushReleases>true</pushReleases>
                        <noDeploy>true</noDeploy>
                    </configuration>
                </plugin>
```
```
mvn jgitflow:release-start
mvn jgitflow:release-finish -Dmaven.javadoc.skip=true -DskipTests=true -Dsquash=false -DpullMaster=true
```
if you have issue with 'conflict with master...' - just merge *master* to *develop*

### maven tomcat plugin 
```
mvn  org.apache.tomcat.maven:tomcat7-maven-plugin:2.2:redeploy -Dmaven.test.skip -Dmaven.tomcat.url=http://host:8080/manager/text -Dtomcat.username=manager -Dtomcat.password=manager
```

%TOMCAT%/conf/tomcat-users.xml:
```
  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <role rolename="manager-jmx"/>
  <role rolename="manager-status"/>
  <role rolename="admin-gui"/>
  <role rolename="admin-script"/>
  <user username="manager" password="manager" roles="manager-gui,manager-script,manager-jmx,manager-status,admin-gui,admin-script"></user>
```

### vert.x project 
> mvn vertx:run
```
            <dependency>
                <groupId>io.vertx</groupId>
                <artifactId>vertx-dependencies</artifactId>
                <version>${vertx.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <plugin>
                <groupId>io.fabric8</groupId>
                <artifactId>vertx-maven-plugin</artifactId>
                <version>${vertx-maven-plugin.version}</version>
                <executions>
                    <execution>
                        <id>vmp</id>
                        <goals>
                            <goal>initialize</goal>
                            <goal>package</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <redeploy>true</redeploy>
                    <jvmArgs>-Djava.net.preferIPv4Stack=true</jvmArgs>
                </configuration>
            </plugin>
```

### spring boot project 
> mvn spring-boot:run
```
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
```

### fabric8 with Vert.x deployment ( Source-to-Image S2I )
[fabric8 source code and examples](https://github.com/fabric8io/fabric8-maven-plugin/tree/master/samples)
[maven fabric8 documentation](http://maven.fabric8.io/)
> mvn fabric8
> mvn fabric8:deploy
> mvn fabric8:undeploy
```
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
                            <exclude>vertx-health-check</exclude>
                        </excludes>
                    </enricher>
                    <generator>
                        <includes>
                            <include>vertx</include>
                        </includes>
                        <config>
                            <vertx>
                                <from>registry.access.redhat.com/redhat-openjdk-18/openjdk18-openshift:1.1</from>
                            </vertx>
                        </config>
                    </generator>
                </configuration>
            </plugin>
```

### fabric8 with SpringBoot deployment ( Source-to-Image S2I )
> mvn fabric8
> mvn fabric8:deploy
> mvn fabric8:undeploy
```
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
```

### wildfly project
> mvn wildfly-swarm:run
```
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
```

### fabric8 with WildFly, openshift with WildFly, WildFly Swarm ( Source-to-Image S2I )
> mvn fabric8
> mvn fabric8:deploy
> mvn fabric8:undeploy
```
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
```
### set version of source code 
```
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
```

### maven war plugin
```
    <plugin>
        <artifactId>maven-war-plugin</artifactId>
        <version>3.1.0</version>
        <configuration>
          <failOnMissingWebXml>false</failOnMissingWebXml>
        </configuration>
      </plugin>
```

### maven exec plugin
> mvn exec:java
```
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
```

### copy into package additional resources 
```
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
```

### sonar plugin
%Maven%/conf/settings.xml
```
		<profile>
			<id>sonar</id>
			<activation>
				<activeByDefault>true</activeByDefault>
			</activation>
			<properties>
				<sonar.jdbc.url>
				  jdbc:mysql://localhost:3306/sonar_schema?useUnicode=true&amp;characterEncoding=utf8
				</sonar.jdbc.url>
				<sonar.jdbc.driverClassName>com.mysql.jdbc.Driver</sonar.jdbc.driverClassName>
				<sonar.jdbc.username>root</sonar.jdbc.username>
				<sonar.jdbc.password></sonar.jdbc.password>

				<sonar.host.url>
				  http://localhost:9000
				</sonar.host.url>
			</properties>
		</profile>
```
mvn sonar:sonar


### smallest pom.xml, init pom.xml, start pom.xml
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>workplace</name>
  <url>http://maven.apache.org</url>
  
  <groupId>com.cherkashyn.vitalii.startup.searchcorrector</groupId>
  <artifactId>workplace</artifactId>
	
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```


# Archetypes
## The desired archetype does not exist
```sh
# https://repository.apache.org/content/groups/snapshots-group/org/apache/camel/archetypes/3.4.0-SNAPSHOT/maven-metadata.xml
#   -DarchetypePackaging=pom \
#   -Dpackaging=pom \
mvn archetype:generate\
  -X \
  -DarchetypeGroupId=org.apache.camel \
  -DarchetypeVersion=3.4.0-SNAPSHOT \
  -DarchetypeArtifactId=archetypes \
  -DarchetypeRepository=https://repository.apache.org/content/groups/snapshots-group | grep resolution

# or original 
mvn archetype:generate \
  -DarchetypeGroupId=org.apache.camel.archetypes \
  -DarchetypeArtifactId=camel-archetype-java \
  -DarchetypeVersion=3.4.0-SNAPSHOT \
  -DarchetypeRepository=https://repository.apache.org/content/groups/snapshots-group
```

```sh
vim $HOME/.m2/repository/archetype-catalog.xml
```

```xml
# https://maven.apache.org/archetype/archetype-models/archetype-catalog/archetype-catalog.html
<?xml version="1.0" encoding="UTF-8"?>
<archetype-catalog  xmlns="http://maven.apache.org/plugins/maven-archetype-plugin/archetype-catalog/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/plugins/maven-archetype-plugin/archetype-catalog/1.0.0 http://maven.apache.org/xsd/archetype-catalog-1.0.0.xsd">
  <archetypes>
    <archetype>
        <groupId>org.apache.camel</groupId>
        <artifactId>archetypes</artifactId>
        <version>3.4.0-SNAPSHOT</version>
        <repository>https://repository.apache.org/content/groups/snapshots-group</repository>
        <description>Apache camel archetype</description>
    </archetype>
  </archetypes>
</archetype-catalog>
```
