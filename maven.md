## Repositories
* https://mvnrepository.com/  

## maven scope explanations
![maven scopes](https://user-images.githubusercontent.com/8113355/143955154-eab903a5-a069-4773-a217-e0472c0c621f.png)


## create project, init project, new project

### example of creating project create project empty 
```
mvn archetype:generate -DgroupId=com.cherkashyn.vitalii.startup.searchcorrector -DartifactId=searchcorrector -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

### example of creating project create web project maven create Java web project empty web project
``` 
mvn archetype:generate -DgroupId=com.cherkashyn.vitalii.startup.searchcorrector -DartifactId=workplace -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
mvn archetype:generate -DgroupId=com.cherkashyn.vitalii.smava.onsite -DartifactId=soap-calculator -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
```

### for creating Eclipse Web project ( change pom.xml:packaging to "war" ) :
``` 
mvn eclipse:eclipse -Dwtpversion=2.0
```

### build with many threads
```sh
mvn -T 1C clean install # 1 per thread 
mvn -T 4 clean install # 4 threads
```

### build sub-modules with parent-dependencies
```sh
mvn -am ...
mvn --also-make ...
```

### build in another folder specify project folder specify project directory
```sh
mvn -f $DIR_PROJECT/data-manager/pom.xml
```

### list of all modules
```
mvn org.qunix:structure-maven-plugin:modules
```


### build only one module, build one module, single module build
```sh
# mvn --projects common/common-utils clean install
mvn -pl common/common-utils clean install
```
or build with all dependencies
```sh
mvn --threads 2C --projects common/common-utils -am clean install
```

### build without module skip module
```sh
mvn -f $DIR_PROJECT/pom.xml clean install -pl -:processing-common -Dmaven.test.skip=true -DskipTests 
mvn -f $DIR_PROJECT/pom.xml clean install -pl '-:processing-common,-:processing-e2e' -Dmaven.test.skip=true -DskipTests 
mvn clean package -pl '!:processing-common,!:processing-mapr-ojai-common'
```

### continue to build after interruption 
```sh
mvn --resume-from :processing-common install -Dmaven.test.skip=true -DskipTests 
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
```
-DforkCount=0 -DreuseForks=false -DforkMode=never 
```

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

### using another local repo
```sh
mvn clean package --batch-mode --no-transfer-progress -Dmaven.repo.local=/my/own/path/.m2/repository
```

### security settings
~/.m2/settings.xml
[artifactory token generation](https://github.com/cherkavi/cheat-sheet/blob/master/artifactory.md)
```
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.1.0 http://maven.apache.org/xsd/settings-1.1.0.xsd">

    <servers>
        <server>
            <id>data-manager-releases</id>
            <username>cherkavi</username>
	    <password>eyJ2ZXIiO....</password>
        </server>
    </servers>
</settings>
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
!!! important - if your password contains symbols like $,&... pls, use escape characters like: &amp;
or
```sh
mvn compile -Dhttp.proxyHost=10.10.0.100 -Dhttp.proxyPort=8080 -Dhttp.nonProxyHosts=localhost|127.0.0.1 -Dhttp.proxyUser=baeldung -Dhttp.proxyPassword=changeme
```

## Plugins:
* http://www.mojohaus.org/build-helper-maven-plugin/
  > http://www.mojohaus.org/build-helper-maven-plugin/usage.html
  build-helper:add-source Add more source directories to the POM.
  * build-helper:add-test-source Add test source directories to the POM.
  * build-helper:add-resource Add more resource directories to the POM.
  * build-helper:add-test-resource Add test resource directories to the POM.
  * build-helper:attach-artifact Attach additional artifacts to be installed and deployed.
  * build-helper:maven-version Set a property containing the current version of maven.
  * build-helper:regex-property Sets a property by applying a regex replacement rule to a supplied value.
  * build-helper:regex-properties Sets a property by applying a regex replacement rule to a supplied value.
  * build-helper:released-version Resolve the latest released version of this project.
  * build-helper:parse-version Parse the version into different properties.
  * build-helper:remove-project-artifact Remove project's artifacts from local repository.
  * build-helper:reserve-network-port Reserve a list of random and unused network ports.
  * build-helper:local-ip Retrieve current host IP address.
  * build-helper:hostname Retrieve current hostname.
  * build-helper:cpu-count Retrieve number of available CPU.
  * build-helper:timestamp-property Sets a property based on the current date and time.
  * build-helper:uptodate-property Sets a property according to whether a file set's outputs are up to date with respect to its inputs.
  * build-helper:uptodate-properties Sets multiple properties according to whether multiple file sets' outputs are up to date with respect to their inputs.
  * build-helper:rootlocation Sets a property which defines the root folder of a multi module build.
* https://www.mojohaus.org/buildnumber-maven-plugin/
  > https://www.mojohaus.org/buildnumber-maven-plugin/usage.html
  * buildnumber:create: Create a build number.
  * buildnumber:create-timestamp: Create a timestamp.
  * buildnumber:create-metadata: Write build properties into file
  * buildnumber:hgchangeset: Create properties for changeSet and changeSetDate from a Mercurial repository.
* https://mavenlibs.com/maven/plugin/net.ju-n.maven.plugins/checksum-maven-plugin
  > checksum digests and output them to individual or summary files
  ```xml
    <plugin>
        <groupId>net.ju-n.maven.plugins</groupId>
        <artifactId>checksum-maven-plugin</artifactId>
        <version>${maven-checksum-plugin.version}</version>
        <executions>
            <execution>
                <goals>
                    <goal>artifacts</goal>
                </goals>
            </execution>
        </executions>
        <configuration>
            <failOnError>false</failOnError>
        </configuration>
    </plugin>
  ```
* http://coderplus.github.io/copy-rename-maven-plugin/
  > http://coderplus.github.io/copy-rename-maven-plugin/usage.html
  * copy-rename:copy Copy files during maven build.
  * copy-rename:rename Rename files or directories during maven build.
* [dependency-check-core to detect publicly disclosed vulnerabilities, owasp](https://jeremylong.github.io/DependencyCheck/dependency-check-maven/index.html)
* [download files](https://github.com/maven-download-plugin/maven-download-plugin)
* [execute system and Java programs](https://www.mojohaus.org/exec-maven-plugin/)
  > https://www.mojohaus.org/exec-maven-plugin/usage.html
* [find security bugs](https://spotbugs.readthedocs.io/en/latest/maven.html)
  > old plugin: https://gleclaire.github.io/findbugs-maven-plugin/
* [downloads/installs Node and NPM locally for your project, runs npm install, and then any combination of Bower, Grunt, Gulp, Jspm, Karma, or Webpack.](https://github.com/eirslett/frontend-maven-plugin)
* [run gatling tests](https://gatling.io/docs/gatling/reference/current/extensions/maven_plugin/)
* [git commit id](https://github.com/git-commit-id/git-commit-id-maven-plugin)
* [Provides support for execution, compilation and other facets of Groovy development](https://groovy.github.io/gmaven/groovy-maven-plugin/)
* [java COde COverage plugin test coverage reports ](https://mvnrepository.com/artifact/org.jacoco/jacoco-maven-plugin)
* [building Docker and OCI images for your Java applications](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin)
* [convert json schema to pojo](https://mvnrepository.com/artifact/org.jsonschema2pojo/jsonschema2pojo-maven-plugin)
* [view and transfer resources between repositories](http://www.mojohaus.org/wagon-maven-plugin/)
  > http://www.mojohaus.org/wagon-maven-plugin/usage.html
* [replace text in file with another value](https://mvnrepository.com/artifact/com.google.code.maven-replacer-plugin/replacer)
* [scoverage code coverage library](https://github.com/scoverage/scoverage-maven-plugin)
* [uses Protocol Buffer Compiler (protoc) tool to generate Java source files from .proto (protocol buffer definition) files for the specified projec](https://www.xolstice.org/protobuf-maven-plugin/)
  > https://www.xolstice.org/protobuf-maven-plugin/usage.html
  * protobuf:compile compiles main .proto definitions into Java files and attaches the generated Java sources to the project.
  * protobuf:test-compile compiles test .proto definitions into Java files and attaches the generated Java test sources to the project.
  * protobuf:compile-cpp compiles main .proto definitions into C++ files and attaches the generated C++ sources to the project.
  * protobuf:test-compile-cpp compiles test .proto definitions into C++ files and attaches the generated C++ test sources to the project.
  * protobuf:compile-python compiles main .proto definitions into Python files and attaches the generated Python sources to the project.
  * protobuf:test-compile-python compiles test .proto definitions into Python files and attaches the generated Python test sources to the project.
  * protobuf:compile-csharp compiles main .proto definitions into C# files and attaches the generated C# sources to the project.
  * protobuf:test-compile-csharp compiles test .proto definitions into C# files and attaches the generated C# test sources to the project.
  * protobuf:compile-js compiles main .proto definitions into JavaScript files and attaches the generated JavaScript sources to the project.
  * protobuf:test-compile-js compiles test .proto definitions into JavaScript files and attaches the generated JavaScript test sources to the project.
  * protobuf:compile-javanano uses JavaNano generator (requires protobuf compiler version 3 or above) to compile main .proto definitions into Java files and attaches the generated Java sources to the project.
  * protobuf:test-compile-javanano uses JavaNano generator (requires protobuf compiler version 3 or above) to compile test .proto definitions into Java files and attaches the generated Java test sources to the project.
  * protobuf:compile-custom compiles main .proto definitions using a custom protoc plugin.
  * protobuf:test-compile-custom compiles test .proto definitions using a custom protoc plugin.
* [code formatter](https://github.com/diffplug/spotless)
* [generate openapi (swagger) documentation](https://github.com/openapi-tools/swagger-maven-plugin)
* [build server stubs and client SDKs directly from a Swagger defined RESTful AP](https://swagger.io/docs/open-source-tools/swagger-codegen/)
* [package executable jar or war archives, run Spring Boot applications, generate build information and start your Spring Boot application prior to running integration tests](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/maven-plugin/)
  > https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/maven-plugin/usage.html
  * spring-boot:run runs your Spring Boot application.
  * spring-boot:repackage repackages your jar/war to be executable.
  * spring-boot:start and spring-boot:stop to manage the lifecycle of your Spring Boot application (i.e. for integration tests).
  * spring-boot:build-info generates build information that can be used by the Actuator.  
* [generate json and yaml OpenAPI description during build time](https://springdoc.org/plugins.html)
* [run ant tasks](https://maven.apache.org/plugins/maven-antrun-plugin/usage.html)
* [combine all project output into a single archive distributed file](https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-assembly-plugin)
* [performs Checkstyle analysis and generates an HTML report on any violations that Checkstyle finds](https://maven.apache.org/plugins/maven-checkstyle-plugin/checkstyle-mojo.html)
* [removes files generated at build-time in a project's directory.](https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-clean-plugin)
* [It can copy and/or unpack artifacts from local or remote repositories to a specified location](https://maven.apache.org/plugins/maven-dependency-plugin/usage.html)
  * dependency:analyze analyzes the dependencies of this project and determines which are: used and declared; used and undeclared; unused and declared.
  * dependency:analyze-dep-mgt analyzes your projects dependencies and lists mismatches between resolved dependencies and those listed in your dependencyManagement section.
  * dependency:analyze-only is the same as analyze, but is meant to be bound in a pom. It does not fork the build and execute test-compile.
  * dependency:analyze-report analyzes the dependencies of this project and produces a report that summarises which are: used and declared; used and undeclared; unused and declared.
  * dependency:analyze-duplicate analyzes the <dependencies/> and <dependencyManagement/> tags in the pom.xml and determines the duplicate declared dependencies.
  * dependency:build-classpath tells Maven to output the path of the dependencies from the local repository in a classpath format to be used in java -cp. The classpath file may also be attached and installed/deployed along with the main artifact.
  * dependency:copy takes a list of artifacts defined in the plugin configuration section and copies them to a specified location, renaming them or stripping the version if desired. This goal can resolve the artifacts from remote repositories if they don't exist in either the local repository or the reactor.
  * dependency:copy-dependencies takes the list of project direct dependencies and optionally transitive dependencies and copies them to a specified location, stripping the version if desired. This goal can also be run from the command line.
  * dependency:display-ancestors displays all ancestor POMs of the project. This may be useful in a continuous integration system where you want to know all parent poms of the project. This goal can also be run from the command line.
  * dependency:get resolves a single artifact, eventually transitively, from a specified remote repository.
  * dependency:go-offline tells Maven to resolve everything this project is dependent on (dependencies, plugins, reports) in preparation for going offline.
  * dependency:list alias for resolve that lists the dependencies for this project.
  * dependency:list-classes displays the fully package-qualified names of all classes found in a specified artifact.
  * dependency:list-repositories displays all project dependencies and then lists the repositories used.
  * dependency:properties set a property for each project dependency containing the to the artifact on the file system.
  * dependency:purge-local-repository tells Maven to clear dependency artifact files out of the local repository, and optionally re-resolve them.
  * dependency:resolve tells Maven to resolve all dependencies and displays the version. JAVA 9 NOTE: will display the module name when running with Java 9.
  * dependency:resolve-plugins tells Maven to resolve plugins and their dependencies.
  * dependency:sources tells Maven to resolve all dependencies and their source attachments, and displays the version.
  * dependency:tree displays the dependency tree for this project.
  * dependency:unpack like copy but unpacks.
  * dependency:unpack-dependencies like copy-dependencies but unpacks.  
* [add your artifact(s) to a remote repository](https://maven.apache.org/plugins/maven-deploy-plugin/usage.html)
* [control certain environmental constraints such as Maven version, JDK version and OS family ](https://maven.apache.org/enforcer/maven-enforcer-plugin/usage.html)
  * enforcer:enforce executes rules for each project in a multi-project build.
  * enforcer:display-info display the current information as detected by the built-in rules.
* [run integration tests](https://maven.apache.org/surefire/maven-failsafe-plugin/usage.html)
* [add artifacts to local repository](https://maven.apache.org/plugins/maven-install-plugin/plugin-info.html)
* [build jars](https://maven.apache.org/plugins/maven-jar-plugin/)
* [sign jars](https://maven.apache.org/plugins/maven-jarsigner-plugin/)
* [Repackages the project classes together with their dependencies into a single uber-jar](https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-shade-plugin)
* [copying of project resources to the output directory](https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-resources-plugin)
* [retrieve JARs of resources from remote repositories, process those resources, and incorporate them into JARs you build with Maven](https://maven.apache.org/plugins/maven-remote-resources-plugin/usage.html)
* [execute the unit tests ](https://maven.apache.org/surefire/maven-surefire-plugin/usage.html)

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
example of project structure ( otherwise your custom classes woun't be added )
```
├── pom.xml
└── src
    ├── main
    │   └── java
    │       └── com
    │           └── cherkashyn
    │               └── vitalii
    │                   └── tools
    │                       ├── App.java
    │                       └── JarExtractor.java
    └── test
        └── java
            └── com
                └── cherkashyn
                    └── vitalii
                        └── tools
                            ├── AppTest.java
                            └── JarExtractorTest.java

```

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <packaging>jar</packaging>
    <version>1.0</version>
    <name>db-checker</name>

    <groupId>com.cherkashyn.vitalii.db</groupId>
    <artifactId>checker</artifactId>

    <dependencies>

        <!-- https://mvnrepository.com/artifact/org.postgresql/postgresql -->
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <version>42.2.12</version>
        </dependency>

    </dependencies>

    <build>
        <testResources>
            <testResource>
                <directory>src/test/resources</directory>
            </testResource>
        </testResources>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
            </resource>
        </resources>
        <plugins>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-assembly-plugin</artifactId>
                <!-- version>2.5.4</version -->
                <configuration>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                    <archive>
                        <!-- manifestFile>${project.basedir}/src/main/resources/META-INF/MANIFEST.MF</manifestFile -->
                        <manifest>
                            <mainClass>com.cherkashyn.vitalii.db.PostgreCheck</mainClass>
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
        </plugins>
    </build>

</project>
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
```sh
mvn sonar:sonar
```

### docker plugin 
```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <executions>
                    <!-- override build docker image with additional arguments -->
                    <execution>
                        <id>docker-build</id>
                        <configuration>
                            <environmentVariables>
                                <DOCKER_BUILDKIT>${enable-docker-build-kit}</DOCKER_BUILDKIT>
                            </environmentVariables>
                            <arguments combine.children="append">
                                <argument>--secret</argument>
                                <argument>id=netrc,src=${netrc-path}</argument>
                            </arguments>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```
```sh
# Dockerfile should exists alongside with pom.xml
mvn verify -Denable-docker-build
```


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

### [surefire plugin](https://maven.apache.org/surefire/maven-surefire-plugin/)
```xml
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration>
                    <classpathDependencyExcludes>
                        <classpathDependencyExclude>org.apache.logging.log4j:log4j-slf4j-impl</classpathDependencyExclude>
                    </classpathDependencyExcludes>
                </configuration>
            </plugin>
```

### [failsafe plugin](https://maven.apache.org/surefire/maven-failsafe-plugin/)
```xml
     <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-failsafe-plugin</artifactId>
                <configuration>
                    <classpathDependencyExcludes>
                        <classpathDependencyExclude>org.apache.logging.log4j:log4j-slf4j-impl</classpathDependencyExclude>
                    </classpathDependencyExcludes>
                </configuration>
            </plugin>
```

### [multiple java versions](https://maven.apache.org/guides/mini/guide-using-toolchains.html)
```xml
<?xml version="1.0" encoding="UTF8"?>
<toolchains>
    <toolchain>
        <type>jdk</type>
        <provides>
            <id>jdk8</id>
            <version>8</version>
            <vendor>openjdk</vendor>
        </provides>
        <configuration>
            <jdkHome>/path/to/jdk8</jdkHome>
        </configuration>
    </toolchain>
    <toolchain>
        <type>jdk</type>
        <provides>
            <id>jdk13</id>
            <version>13</version>
            <vendor>openjdk</vendor>
        </provides>
        <configuration>
            <jdkHome>/path/to/jdk13</jdkHome>
        </configuration>
    </toolchain>
</toolchains>
```
```xml
<properties>
  <jdk.version>8</jdk.version>
</properties>
```
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-toolchains-plugin</artifactId>
    <version>${maven-toolchains-plugin.version}</version>
    <executions>
        <execution>
            <goals>
                <goal>toolchain</goal>
            </goals>
        </execution>
    </executions>
    <configuration>
        <toolchains>
            <jdk>
                <version>${jdk.version}</version>
            </jdk>
        </toolchains>
    </configuration>
</plugin>
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

## find all plugins in project, search for all plugings in project
```sh
for each_file in `find . -name pom.xml | grep -v target`; do
    # cat $each_file | grep plugins
    AMOUNT_OF_PLUGINS=`cat $each_file | xpath -e "/project/build/plugins/plugin/artifactId | /project/profiles/profile/build/plugins/plugin/artifactId" 2>1 | wc -l`
    if [[ $AMOUNT_OF_PLUGINS > 0 ]]; then
        echo "---"
        echo $each_file"  "$AMOUNT_OF_PLUGINS 
        cat $each_file | xpath -e "/project/build/plugins/plugin/artifactId | /project/profiles/profile/build/plugins/plugin/artifactId" 2>1 | grep -v "^-- NODE"
    fi
done
# cat out.txt | grep -v "\-\-\-" | grep -v "pom.xml" | sort | uniq
```
