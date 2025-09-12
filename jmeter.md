# [jmeter](https://jmeter.apache.org/download_jmeter.cgi)

## run jmeter
```sh
export BEARER_TOKEN=$(oauth-int-m2m-bearer-token $CLIENT_ID $CLIENT_SECRET)
echo "$BEARER_TOKEN"

export ARCHIVING_HOST=archiving.apps.4wm-int.eu-central-1.aws.cloud

export USER_ID=MICROSERVICE_USER

jmeter -n -t archiving_post_test_plan.jmx -JARCHIVING_HOST="$ARCHIVING_HOST" -JBEARER_TOKEN="$BEARER_TOKEN" -JUSER_ID="$USER_ID" -j out.log

# skip creating jmeter.log file
jmeter -n -t archiving_post_test_plan.jmx -JARCHIVING_HOST="$ARCHIVING_HOST" -JBEARER_TOKEN="$BEARER_TOKEN" -JUSER_ID="$USER_ID" -j /dev/null

# run with log output in stdout and debug level 
jmeter -n -t archiving_post_test_plan.jmx -JARCHIVING_HOST="$ARCHIVING_HOST" -JBEARER_TOKEN="$BEARER_TOKEN" -JUSER_ID="$USER_ID" -j out.log -Jlog_level.org.apache.jmeter.protocol.http=DEBUG 

# run with log output in stdout and debug level and logging responses (jmeter.properties)
jmeter -n -t archiving_post_test_plan.jmx -JARCHIVING_HOST="$ARCHIVING_HOST" -JBEARER_TOKEN="$BEARER_TOKEN" -JUSER_ID="$USER_ID" -j out.log -Jlog_level.org.apache.jmeter.protocol.http=DEBUG -l results.log
```

## jmeter test plan
### 5 time call with 1 thread
```xml
            <elementProp name="ThreadGroup.main_controller" elementType="LoopController" guiclass="LoopControlPanel" testclass="LoopController" testname="Loop Controller">
                <boolProp name="LoopController.continue_forever">false</boolProp>
                <stringProp name="LoopController.loops">5</stringProp>
            </elementProp>
            
            <stringProp name="ThreadGroup.num_threads">1</stringProp>
```
### 120 sec 50 threads
```xml
            <elementProp name="ThreadGroup.main_controller" elementType="LoopController" guiclass="LoopControlPanel" testclass="LoopController" testname="Loop Controller">
                <boolProp name="LoopController.continue_forever">true</boolProp>
                <stringProp name="LoopController.loops">5</stringProp>
            </elementProp>
            
            <stringProp name="ThreadGroup.num_threads">50</stringProp>
            <stringProp name="ThreadGroup.duration">120</stringProp>
            
            <stringProp name="ThreadGroup.ramp_time">1</stringProp>
            <boolProp name="ThreadGroup.scheduler">true</boolProp>            
```

## collaboration with running jmeter instance
> only with jmeter-server ( not with jmeter -n instance )
```sh
echo Shutdown | nc localhost 4445
echo StopTestNow | nc localhost 4445
echo ThreadDump | nc localhost 4445
echo HeapDump | nc localhost 4445 -O 8
```
