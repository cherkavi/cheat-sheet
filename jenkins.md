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
java -jar jenkins.war --httpPort=8080 --useJmx 
> ~/.jenkins/secrets/

### plugins
