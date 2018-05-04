### links 
* [official book](https://jenkins.io/doc/book/pipeline/syntax/)
* [official github](https://github.com/jenkinsci)
* [pipeline description](https://jenkins.io/doc/book/pipeline/syntax/)
* [pipeline](https://jenkins.io/solutions/pipeline/)
* [user handbook](https://jenkins.io/user-handbook.pdf)
* [BlueOcean](https://jenkins.io/doc/book/blueocean/)

### installation on debian
* wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
* echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
* sudo apt-get update
* sudo apt-get install jenkins
* sudo systemctl start jenkins
* sudo systemctl status jenkins

### manual start
[manual](https://wiki.jenkins.io/display/JENKINS/Starting+and+Accessing+Jenkins)
```
java -jar jenkins.war --httpPort=8080 --useJmx 
~/.jenkins/secrets/
```

### restart jenkins
* {jenkins-url}/safeRestart
* {jenkins-url}/restart
* java -jar /var/cache/jenkins/war/WEB-INF/jenkins-cli.jar -s {jenkins-url} safe-restart 

### stop jenkins
{jenkins-url}/exit

### Script how to find all accessible methods

### Script Console ( Manage Jenkins )
```
Thread.getAllStackTraces().keySet().each() {
  t -> if (t.getName()=="YOUR THREAD NAME" ) {   t.interrupt();  }
}
Jenkins.instance.getItemByFullName("Brand Server/develop").getBuildByNumber(614).finish(hudson.model.Result.ABORTED, new java.io.IOException("Aborting build"));
Jenkins.instance.getItemByFullName("Brand Server/develop").getBuildByNumber(614).doKill();
```

### plugin manual installation
```
copy jpi/hpi file into {JENKINS_HOME/plugins}
```

### plugin manual removing
```
copy jpi/hpi file into {JENKINS_HOME/plugins}
```


### plugins

## Issues
> ERROR: script not yet approved for use
```
http://localhost:9090/scriptApproval/
```

> withMaven(jdk: 'jdk8', maven: 'mvn-325', mavenSettingsConfig: 'paps-maven-settings') {
```
jdk with name "jdk8" should be configured: http://localhost:9090/configureTools/
maven with name "mvn-325" should be configured: http://localhost:9090/configureTools/
```

>Could not find the Maven settings.xml config file id:paps-maven-settings. Make sure it exists on Managed Files
```
plugin should be present
http://localhost:9090/configfiles/
```
