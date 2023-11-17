# Jenkins cheat sheet

## Deployment strategies:
* Delivery ( need approve from the "human" )
* Deployment ( Continuous ) = Delivery (auto approve) + Automation + Event (time,commits,tag...)

## links 
* [jenkins build-in plugins documentation ('commands' like sh)](https://www.jenkins.io/doc/pipeline/steps/)
* [official book](https://jenkins.io/doc/book/pipeline/syntax/)
* [official github](https://github.com/jenkinsci)
* [pipeline description](https://jenkins.io/doc/book/pipeline/syntax/)
* [pipeline](https://jenkins.io/solutions/pipeline/)
* [user handbook](https://jenkins.io/user-handbook.pdf)
* [BlueOcean](https://jenkins.io/doc/book/blueocean/)
* [pipeline examples](https://github.com/jenkinsci/pipeline-examples)
* [continuous delivery patterns](https://continuousdelivery.com/implementing/patterns/)
* [continuous delivery architecture](https://continuousdelivery.com/implementing/architecture/)
* [REST API](https://www.jenkins.io/doc/book/using/remote-access-api/)
  > for all url just try to add: `/api` or `/api/json?pretty=true`
* [jenkins scheduler build periodically](https://www.lenar.io/jenkins-schedule-build-periodically/)
  * each minute: `* * * * *`

## alternatives
* [Gitlab community :: Gitlab]()
* [TeamCity :: JetBrains]()
* [Team Fundation Server :: Microsoft]()
* [GoCD](https://www.gocd.org/)  
* [buildbot](buildbot.net)  

## alternatives in cloud ( SaaS )
* [Bamboo]()
* [Circle CI](./circleci-cheat-sheet.md)
* [Travis CI]()
* [Gitlab]()

## DSL
* [jenkins dsl source code](https://github.com/jenkinsci/job-dsl-plugin)
* [jenkins plugin documentation](https://jenkinsci.github.io/job-dsl-plugin/)

## installation on debian
* wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
* echo deb https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
* sudo apt-get update
* sudo apt-get install jenkins
* sudo systemctl start jenkins
* sudo systemctl status jenkins

## installation master-slave
![master-slave](https://i.postimg.cc/TwWL9QKK/jenkins-master-slave.png)  
![master-slave](https://i.postimg.cc/J72b0b13/jenkins-master-slave.png)
master-slave creation process for OCP  
![master-slave-ocp-creation](https://user-images.githubusercontent.com/8113355/183613775-8859cf57-c2f4-4957-8c3e-a07ee015acbc.png)


## manual clean up
rm -rf .jenkins/caches/*
rm -rf .jenkins/workspace/*

## manual start manual installation
[jenkins manual how to](https://wiki.jenkins.io/display/JENKINS/Starting+and+Accessing+Jenkins)  
[jenkins war](https://www.jenkins.io/doc/book/installing/war-file/)  
[jenkins war download](https://www.jenkins.io/download/)  
[jenkins docker image](https://hub.docker.com/r/jenkins/jenkins/)  
[jenkins docker documentation](https://github.com/jenkinsci/docker/blob/master/README.md)
```
java -jar jenkins.war --httpPort=8080 --useJmx 
~/.jenkins/secrets/
```

## how to know version
(/var/lib/jenkins)
(/opt/webconf/var/lib/jenkins)
config.xml
<version>1.599</version>

## restart jenkins
* {jenkins-url}/safeRestart
* {jenkins-url}/restart
* java -jar /var/cache/jenkins/war/WEB-INF/jenkins-cli.jar -s {jenkins-url} safe-restart 

## stop jenkins
{jenkins-url}/exit

## connect to jenkins
```sh
JENKINS_HOST=jenkins-stg.vantage.com
JENKINS_USER=cherkavi

JENKINS_URL=https://$JENKINS_HOST
```

### [jenkins connect via cli](https://www.jenkins.io/doc/book/managing/cli/)
```sh
# curl -Lv $JENKINS_URL/login 2>&1  | grep -i 'cli-port'
# wget $JENKINS_URL/jnlpJars/jenkins-cli.jar
java -jar jenkins-cli.jar -noCertificateCheck -s $JENKINS_URL help

java -jar jenkins-cli.jar -s $JENKINS_URL -webSocket -auth $JENKINS_USER:$JENKINS_API_TOKEN help
java -jar jenkins-cli.jar -s $JENKINS_URL -noCertificateCheck -auth $JENKINS_USER:$JENKINS_API_TOKEN help
```

## connect via ssh
```sh
curl -Lv $JENKINS_URL/login 2>&1  | grep -i 'x-ssh-endpoint'
# security settings: add public ssh 
# $JENKINS_URL/user/$JENKINS_USER/configure -> SSH Public Keys

# x-jenkins-cli-port: 50000
ssh -l $JENKINS_USER -p 50000 $JENKINS_HOST help

java -jar jenkins-cli.jar -s $JENKINS_URL -ssh -user $JENKINS_USER -i ~/.ssh/id_rsa -logger FINE
```


### connect via ssh
```sh
# security settings: add public ssh 
# $JENKINS_URL/user/$JENKINS_USER/configure -> SSH Public Keys

curl -Lv $JENKINS_URL/login 2>&1  | grep -i 'x-ssh-endpoint'
ssh -l $JENKINS_USER -p 50000 $JENKINS_HOST help
```

## collaboration between steps, passing data between steps
1. Set your variable export myenv=value1 and read in afterwards
2. print to file `echo $START > env_start.txt` and read it afterwards `START=$(cat env_start.txt)`

## Script Console ( Manage Jenkins )
```
Thread.getAllStackTraces().keySet().each() {
  t -> if (t.getName()=="YOUR THREAD NAME" ) {   t.interrupt();  }
}
Jenkins.instance.getItemByFullName("Brand Server/develop").getBuildByNumber(614).finish(hudson.model.Result.ABORTED, new java.io.IOException("Aborting build"));
Jenkins.instance.getItemByFullName("Brand Server/develop").getBuildByNumber(614).doKill();
```

## jenkins simple pipeline
```jenkins
node {
 	stage("Step #1") {
		echo "first bite"
	}

	stage("Step #2") {
		sh "ansible --version"
	}
}
```
```jenkins
def saveToFileUrl(gitRepoUser, gitRepoName, gitFilePath, localFileName){
    final String gitUrl="https://github.net"
    String gitFileDescription = ""
    withCredentials([usernamePassword(credentialsId: 'xxxxx', passwordVariable: 'gitToken', usernameVariable: 'gitUser')]) {
        gitFileDescription = sh(script: "curl -u ${gitUser}:${gitToken} ${gitUrl}/api/v3/repos/${gitRepoUser}/${gitRepoName}/contents/${gitFilePath} | grep download_url", returnStdout: true).trim()
    }   
    
    final String urlResponse = sh(script: "curl -s ${gitFileDescription.split(" ")[1].replaceAll('"','').replaceAll(',','')}", returnStdout: true).trim()
    writeFile file: localFileName, text: urlResponse
    return urlResponse    
}

pipeline {
    agent any

    stages {
        stage('download execution script') {
            steps{
                script{
                    env.TMP_FILE_NAME = createTempFile();
                    saveToFileUrl(getGitRepoUser(), getGitRepoName(), getGitPath(), "${env.TMP_FILE_NAME}")                
                }
            }
        }        
        stage('execute script REST API test') {
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'yyyyy', passwordVariable: 'pass', usernameVariable: 'user')]) {
                        env.D_USER=user
                        env.D_PASS=pass
                        def exitStatus=sh(script: "sh ${env.TMP_FILE_NAME}", returnStatus: true)
                        evaluateResult(exitStatus)
                    }   
                }
            }
        }
    }
}
```
with credential alternative 
```groovy
artifactoryCredential = string(credentialsId: '4d18-aaabb-4cdffaf348', variable: 'ARTIFACTORY_TOKEN')
withCredentials([artifactoryCredential]){
  sh "echo $ARTIFACTORY_TOKEN"
}
```

name of the build and using parameters
```
pipeline {
    agent any

    stages {
        stage('check input boolean parameter') {
            when {
                expression { params.FAIL }
            }
            steps {
                sh 'cat /etc/shadow'
            }
        }
    }
    post{
        success {
            script {
            currentBuild.displayName = "$BUILD_NUMBER - Successfull Build"
            currentBuild.description = "OK"
	    currentBuild.result='SUCCESS'
            }
        }
        failure {
            script {
            currentBuild.displayName = "$BUILD_NUMBER - Failed Build"
            currentBuild.description = "NOT OK"
	    currentBuild.result='FAILURE'
            }
        }
    }
}
```
use "system groovy script" for updating description with multisteps 
```sh
echo "my description" > job_description.txt
```
```sh
def currentBuild = Thread.currentThread().executable
def workspace = build.workspace
def description = new File("${workspace}/job_describtion.txt").text
currentBuild.setDescription(description)
```


## jenkins read http write file from http to scp
```
stage("Step #3") {
		final String url="https://api.ipify.org"
		final String urlResponse = sh(script: "curl -s $url", returnStdout: true).trim()
		final String outputFile = "/var/jenkins_home/my-ip-address.txt"
		writeFile file: outputFile, text: urlResponse

	withCredentials([usernamePassword(credentialsId: '222-333-444', passwordVariable: 'MY_SSH_PASS', usernameVariable: 'MY_SSH_USER')]) {
		final String host="localhost"
		final String outputFileName="my-host-ip"
		
		// final String command="sshpass -p " +MY_SSH_PASS+ " scp " +outputFile+ " " +MY_SSH_USER+ "@" +host+ ":~/" +outputFileName
		// final String command="sshpass -p $MY_SSH_PASS scp $outputFile $MY_SSH_USER@$host:~/$outputFileName"
		final String command="sshpass -p ${MY_SSH_PASS} scp ${outputFile} ${MY_SSH_USER}@${host}:~/${outputFileName}"
		sh(command)
		sh("rm $outputFile")
		}
      }```

### jenkins git timeout
```groovy
checkout([$class: 'GitSCM', branches: [[name: "*/$branch"]], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'GitLFSPull', timeout: 30], [$class: 'CloneOption', depth: 0, timeout: 30], [$class: 'CheckoutOption', timeout: 30]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'a0e5424f-2ffb-', url: '$CC_GIT_CREDENTIAL']]])
    }
```

## jenkins job DSL user input, build with parameters
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

## jenkins job DSL frame for env.BRANCH_NAME<->build step
```
	needToExecuteStage('build', {
            mvn('-U clean package -Dmaven.javadoc.skip=true')
	})

        needToExecuteStage('deploy nexus', {
            mvn('-U deploy -Dmaven.javadoc.skip=true -Dbuild.number=${GIT_BRANCH}-#${BUILD_NUMBER}')
	})

        needToExecuteStage('sonar', {
            mvn('sonar:sonar -Psonar-test')
	})

        needToExecuteStage('integration tests', {
            mvn('install -DskipTests -DskipITs=false -Pintegration-tests,dev -Dheadless=1')
        })

        needToExecuteStage('git label', {
            def newVersion = readVersion()
            print(">-> deploy application with new version: $newVersion")
            sh( script: "git checkout $BRANCH_NAME ")
            def remoteUrl = sh(returnStdout: true, script: "git config --get remote.origin.url ")
            sh( script: "git remote set-url origin $remoteUrl ")
            sh(script: "echo $newVersion > opm-gui/src/main/webapp/META-INF/commit ")
            sh(script: "git rev-parse HEAD >> opm-gui/src/main/webapp/META-INF/commit ")
            sh( script: "git tag -a $newVersion -m 'deployment_jenkins_job' ")
            sshagent (credentials: ['git_jenkins']) {
                 sh("git push --tags $remoteUrl")
            }
            mvn ("versions:set -DnewVersion=$newVersion")
            mvn ("-N versions:update-child-modules")
            mvn ("clean install -DskipTests=true")
		})

        needToExecuteStage('deploy tomcat', {
            mvn('org.apache.tomcat.maven:tomcat7-maven-plugin:2.2:redeploy -Dmaven.tomcat.url=http://v337:9090/manager/text -Dtomcat.username=root -Dtomcat.password=root -DskipTests ')
        })

// -------------------------------------------
def executeStage(needToExecute, stageName, func){
    if(needToExecute){
        stage(stageName){
            func()
        }
    }
}

def needToExecuteStage(stageName, func){
    def decisionTable = [
             'release' : ["build": true,  "deploy nexus": true,  "sonar": false, "integration tests": true,  "git label": true,  "deploy tomcat": false]
            ,'develop' : ["build": true,  "deploy nexus": true,  "sonar": true,  "integration tests": true,  "git label": false, "deploy tomcat": true ]
            ,'feature' : ["build": true,  "deploy nexus": false, "sonar": false, "integration tests": true,  "git label": false, "deploy tomcat": false]
            ,'master'  : ["build": true,  "deploy nexus": true,  "sonar": false, "integration tests": true,  "git label": false, "deploy tomcat": false]
,'integration-test': ["build": true,  "deploy nexus": true,  "sonar": false, "integration tests": true,  "git label": false, "deploy tomcat": false]
    ]

    def branchName = env.BRANCH_NAME
    if(decisionTable[branchName]!=null){
        executeStage(decisionTable[branchName][stageName], stageName, func)
        return
    }

    for ( def key in decisionTable.keySet()){
        if(branchName.startsWith(key)){
            executeStage(decisionTable[key][stageName], stageName, func)
            return
        }
    }
}
```
## groovy escape quotes
```
String input = "this ain't easy"
String escaped = "'" + input.replaceAll(/'/, /'"'"'/) + "'"
println escaped
// 'this ain'"'"'t easy'
```

```
sh "mycommand --input ${escaped}"
```

## groovy random groovy variables
```groovy
// vairable inside jenkins pipeline

pipeline {
    agent any

    environment {
        TMP_FILE_NAME = "kom.yaml"+"${Math.abs(new Random().nextInt(99999))}"
    }

    stages {
        stage('read from git write to cluster') {
            steps {
                echo "----- ${env.TMP_FILE_NAME}"                    

                script {
                    env.TMP_FILE_NAME2 = "second variable"
                }
                echo "----- ${env.TMP_FILE_NAME2}"                    

            }
        }
    }

}
```

## groovy pipeline show credentials
```groovy
// create pipeline with groovy script

/**
Jenkins Pipeline (create project Pipeline) script for printing out credentials 
# jenkins show credentials 
*/
def show(){
    withCredentials([sshUserPrivateKey(credentialsId: 'xxx-yyy-883a-38881320d606', keyFileVariable: 'data_api_key', passphraseVariable: '', usernameVariable: 'data_api_username')]) {
        return "\n>>>  data_api_key ${data_api_key} \n data_api_username: ${data_api_username}"
    }
    withCredentials([usernamePassword(credentialsId: 'xxx-yyy-a659-b54d73eec29a', passwordVariable: 'database_password', usernameVariable: 'database_user')]) {
        return " \nlogin ${database_user} \npassword ${database_password}"
    }
}

pipeline {
    agent any

    stages {
        stage('show me') {
            steps {
                echo "-----"
                echo show().reverse()
                echo "-----"
            }
        }
    }
}

// # echo "gts-pd-sergtsop" | rev
```

### pipeline with bash script and specific agent
```groovy
pipeline {
    agent { label 'agent-maven-python-git-ansible' }

    stages {
        stage('print message ') {
            steps {
                echo 'Hello World'
            }
        }
        stage('print ansible version'){
            steps {
                sh 'ansible-playbook --version'
            }
        }
    }
}
```

### condition for step
```groovy
...
  stage('Action') {
        if (env.CUSTOM_VARIABLE ==~ /(?i)(Y|YES|T|TRUE|ON|RUN)/) {
	 ...
```

### show accessible environment jenkins variables
$JENKINS_URL/env-vars.html/

## REST API
check connection
```sh
JENKINS_URL=https://jenkins-stg.dpl.org
curl -sg "$JENKINS_URL/api/json?tree=jobs[name,url]" --user $DXC_USER:$DXC_PASS
```
deploy with parameters
```sh
# obtain list of parameters: $JENKINS_URL/job/application/job/data-api/job/deployment/job/deploy-services/api/json?pretty=true
curl $JENKINS_URL/job/application/job/data-api/job/deployment/job/deploy-services/buildWithParameters \
  --user $DXC_USER:$DXC_PASS \
  --data BRANCH_NAME=master \
  --data DESTINATION=stg-6 \
  --data DEBUG=true  \
  --data DEPLOY_DATA_APIDEBUG=true  
```
job information
```sh
curl -sg "$JENKINS_URL/job/application/job/data-portal/job/deployment/job/deploy-from-branch-3/244/api/json" --user $DXC_USER:$DXC_PASS
curl -sg "$JENKINS_URL/job/application/job/data-portal/job/deployment/job/deploy-from-branch-3/244/api/json?tree" --user $DXC_USER:$DXC_PASS
curl -sg "$JENKINS_URL/job/application/job/data-portal/job/deployment/job/deploy-from-branch-3/api/json?tree=allBuilds[number,url]" --user $DXC_USER:$DXC_PASS
```
job full log output
```sh
curl -sg "$JENKINS_URL/job/application/job/data-portal/job/deployment/job/deploy-from-branch-3/244/consoleFull" --user $DXC_USER:$DXC_PASS
```

### sonar management
obtaining
```
wget https://sonarsource.bintray.com/Distribution/sonarqube/
```
start/stop/status
```
/dev/sonar/bin/linux-x86-64/sonar.sh start
/dev/sonar/bin/linux-x86-64/sonar.sh status
/dev/sonar/bin/linux-x86-64/sonar.sh stop
```
sonar
```
url: http://host01:9000
login: admin
passw: admin
```

### print all accessible variables
```
echo sh(returnStdout: true, script: 'env')
```

## plugins
### plugin manual installation
```
copy jpi/hpi file into {JENKINS_HOME/plugins}
```

### plugin manual removing
```
copy jpi/hpi file into {JENKINS_HOME/plugins}
```

### list all accessible plugins
```
https://{jenkins-url}/pluginManager/api/xml?depth=1
```

### [email plugin](https://www.jenkins.io/doc/pipeline/steps/email-ext/)
```groovy
emailext body: mailNotification.toString(), subject: "notification", to: env.mail_recipients, mimeType: "text/plain"
```

### email for bash script
```sh
if [[ -n $status_error ]]; then
    echo "need to be considered"
    exit 1
fi
```
Post-build Actions -> E-Mail Notification -> Send e-mail for every unstable build

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

### Workspace sharing between runs 
execute shell 
```sh
echo "---------------"
ls -la current-date.txt || echo "."
cat current-date.txt || echo "."
readlink -f current-date.txt
pwd
date > current-date.txt
ls -la current-date.txt
echo "---------------"
```
output
```
[EnvInject] - Loading node environment variables.
Building on master in workspace /var/lib/jenkins/jobs/application/jobs/data-api/jobs/test/jobs/test-project/workspace
[workspace] $ /bin/sh -xe /tmp/jenkins908209539931105022.sh
+ echo ---------------
---------------
+ ls -la current-date.txt
-rw-r--r--. 1 1001 root 29 Feb  8 10:08 current-date.txt
+ cat current-date.txt
Tue Feb  8 10:08:39 UTC 2022
+ readlink -f current-date.txt
/var/lib/jenkins/jobs/application/jobs/data-api/jobs/test/jobs/test-project/workspace/current-date.txt
+ pwd
/var/lib/jenkins/jobs/application/jobs/data-api/jobs/test/jobs/test-project/workspace
+ date
+ ls -la current-date.txt
-rw-r--r--. 1 1001 root 29 Feb  8 10:13 current-date.txt
+ echo ---------------
---------------
Finished: SUCCESS
```
