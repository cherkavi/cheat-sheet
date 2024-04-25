# Gradle cheat sheet
## setup 
### [gradle bash completion](https://github.com/gradle/gradle-completion)
```sh
DEST_FOLDER=$HOME_SOFT/gradle/bash_completion
mkdir -p $DEST_FOLDER
curl -LA gradle-completion https://edub.me/gradle-completion-bash -o $DEST_FOLDER/gradle-completion.bash

echo 'source $HOME_SOFT/gradle/bash_completion/gradle-completion.bash' >> ~/.bashrc
```
## gradle commands 
### print all dependencies for project, dependency tree
gradlew dependencies

### execute gradle with specific build.gradle file
gradlew.bat -b migration-job/build.gradle build

### quite output, output without messages
gradlew.bat -q build

### gradle debug
https://docs.gradle.org/current/userguide/build_environment.html
gradle  -Dorg.gradle.debug=true --no-daemon clean

### skip tests
gradlew build -x test
gradlew test --test "com.example.android.testing.blueprint.unit.integrationTests.*"

### execute single test
gradlew test -Dtest.single=< wildcard of test > build

## custom task, run script 
### init groovy project
```sh
gradle init --groovy-application
gradle init --type java-library
```
* java-application
* java-library
* scala-library
* groovy-library
* basic

### execute groovy script
add into build.gradle
```groovy
task runScript (dependsOn: 'classes', type: JavaExec) {
    main = 'App'
    classpath = sourceSets.main.runtimeClasspath
}
```
execute script 
```sh
gradle runtScript
```

## proxy settings
```
gradle build -Dhttp.proxyHost=proxy-host -Dhttp.proxyPort=8080 -Dhttp.proxyUser=q4577777 -Dhttp.proxyPassword=my-password
```
