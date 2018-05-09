### links 
* [official book](https://jenkins.io/doc/book/pipeline/syntax/)
* [official github](https://github.com/jenkinsci)
* [pipeline description](https://jenkins.io/doc/book/pipeline/syntax/)
* [pipeline](https://jenkins.io/solutions/pipeline/)
* [user handbook](https://jenkins.io/user-handbook.pdf)
* [BlueOcean](https://jenkins.io/doc/book/blueocean/)
* [jenkins plugin documentation](https://jenkinsci.github.io/job-dsl-plugin/)

### installation on debian
* wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
* echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
* sudo apt-get update
* sudo apt-get install jenkins
* sudo systemctl start jenkins
* sudo systemctl status jenkins

### manual clean up
rm -rf .jenkins/caches/*
rm -rf .jenkins/workspace/*

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

### jenkins job DSL user input, build with parameters
```

		if (isGitBranch('OPM-integration-test')) {
            stage('candidate-git-label') {
                lastCommit = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()
                print(lastCommit)
                def newVersion = readVersion()
                print("this is new version: $newVersion")
            }
            stage('candidate-deploy') {
                mvn('org.apache.tomcat.maven:tomcat7-maven-plugin:2.2:redeploy -Dmaven.tomcat.url=http://host:9090/manager/text -Dtomcat.username=root -Dtomcat.password=root -DskipTests ')
            }
		}

--------------
def readVersion() {
  try {
    timeout(time: 20, unit: 'MINUTES') {
        def keep = input message: 'New version of application:',
                    parameters: [stringParam(defaultValue: "2018.06.00.00-SNAPSHOT", description: 'new application version', name: 'currentBuildVersion')]
        return keep
    }
  } catch(e) {
    return "2018.06.00.00-SNAPSHOT"
  }
}
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

> RSA key fingerprint is SHA256:xxxxxx
> Are you sure you want to continue connecting (yes/no) ?
> ssh://git@webgit.xxxxxx.de:7999/pportal/commons.git
```
change 
gitHostUrl = "https://user@webgit.xxxx.de/scm"
```
