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
