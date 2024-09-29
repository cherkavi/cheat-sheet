# Tomcat cheat sheet
### Tomcat installation 
* wget from https://tomcat.apache.org/download-80.cgi
* http port:
> file:conf/server.xml  XML:Connector@port
* manager application
* * file:conf/tomcat-users.xml should be able to be readable 
* * file:conf/tomcat-users.xml  ( manager-gui, manager-script )
```  <role rolename="manager"/>
  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <role rolename="manager-jmx"/>
  <role rolename="manager-status"/>
  <role rolename="admin-gui"/>
  <role rolename="admin-script"/>
  <user username="root" password="root" roles="manager,manager-gui,manager-script,manager-jmx,manager-status,admin-gui,admin-script" />
```
* * file:conf/server.xml XML:GlobalNamingResources/Resource@pathname="conf/tomcat-users.xml"
* * file:webapps/manager/META-INF/context.xml XML:Context/Valve@allow - maybe need to comment 
* * check {TOMCAT_HOST}:{TOMCAT_PORT}/manager/html with your username/password

### maven plugin for tomcat:
see previous post about "manager" application, 
see [documentation](http://tomcat.apache.org/maven-plugin-trunk/tomcat7-maven-plugin/redeploy-mojo.html)
``` 
mvn  org.apache.tomcat.maven:tomcat7-maven-plugin:2.2:redeploy -Dmaven.test.skip -Dmaven.tomcat.url=http://{host}:8080/manager/text -Dtomcat.username=root -Dtomcat.password=root
```

### Tomcat fast application start
[simple web servlet application](https://github.com/cherkavi/java-web/tree/master/simple-echo-server)  
```sh
TOMCAT_HOME=/home/soft/tomcat/apache-tomcat-9.0.91
WAR_ASSEMBLED=/home/projects/temp/java-web-echo/target/echo-webapp.war

$TOMCAT_HOME/bin/startup.sh      
cp $WAR_ASSEMBLED $TOMCAT_HOME/webapps
sleep 10
x-www-browser http://localhost:8080/echo-webapp/echo
rm $TOMCAT_HOME/webapps/echo-webapp.war
/home/soft/tomcat/apache-tomcat-9.0.91/bin/shutdown.sh
```

### Tomcat debug
> update file: setenv.sh
* for linux:
```
export "JAVA_OPTS=$JAVA_OPTS -Dcatalina.log.level=INFO -Xmx1024m -Duser.timezone=UTC -Dspring.config.location=apache-tomcat-8.0.41-brandserver/conf/application-cherkavi.yml"
export CATALINA_OPTS="-Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=9009 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false"
```
* for windows:
```
set "JAVA_OPTS=%JAVA_OPTS% -Dcatalina.log.level=INFO -Xmx1024m -Duser.timezone=UTC -Dspring.config.location=C:\soft\tomcat\apache-tomcat-8.0.41-brandserver\conf\brand-application-cherkavi.yml"
set "CATALINA_OPTS=-Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=9009 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false"
```

### Tomcat & Apache
> main config file: 
* httpd.conf
> communication files between Tomcat & Apache ( url route, port to be listen ... )
* mod_jk.conf
* workers.properties
* alias.properties


### Issue
```[INFO] I/O exception (java.net.SocketException) caught when processing request: Connection reset by peer: socket write error ```
check "manager" application, username/password


# Payara ( GlassFish )
## folders
* $PAYARA_ROOT_FOLDER/config/conf.d/
  * 10_global.xml
* $PAYARA_ROOT_FOLDER/config/
  * admin-keyfile
  * passwordfile
  * role-mappings.xml

## properties
```sh
env | grep -i payara
```

## manual init
```sh
./GFserver5 stop --all  
./GFserver5 status

./GFserver5 remove-domain 
./GFserver5 create-domain 
```

## manual redeploy
```sh
./GFserver5 stop --all  
./GFserver5 status

# all deployment files must be inside your deployment folder
./GFserver5 remove-domain
./GFserver5 --verbose restore # even if it was ended with exit code >0

./GFserver5 start --all
./GFserver5 status
```
```sh
## start of the application
./GFserver4 stop -A
# ./GFserver4 kill -A

./GFserver4 remove-domain
./GFserver4 extract-tar -f configuration_20231024.tar
# ./GFserver4 restore

./GFserver4 start -A
./GFserver4 status
```

## commands
```sh
./GFserver4 start -i0
./GFserver4 start --all
./GFserver4 sync
./GFserver4 reconfigure # in case of changes (salt file ... ) 
./GFserver4 -V
./GFserver4 remove-config

## encrypt passwords for configuration ( salt file must be accessible, otherwise just create it with the same content for all nodes )
# config/salt file must exists ( or just create it with random text and put on all nodes ) 
./GFserver4 encrypt-password
```
## issues
### wrong version of config
```sh
./GFserver5 status
# FATAL: gfv4 no longer supported by this script

vim 10_global.xml
# <config-version>v082:20xxxxx</config-version>
# change it to: v100:20xxxxx
```
