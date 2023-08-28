# Artifactory
## REST endpoints
### token generation
```sh
ARTIFACTORY_URL=https://artifactory.sbbgroup.net/artifactory 
USERNAME="cherkavi"
curl -u "$USERNAME" -XPOST "$ARTIFACTORY_URL/api/security/token" -d "username=$USERNAME" -d "scope=member-of-groups:*" -d "expires_in=315360000"
```
plain password should be replaced in files 
```
# ~/.git-credentials
# ~/.m2/settings.xml
```

### [check token](https://jfrog.com/help/r/jfrog-rest-apis/system-info)
```sh
curl -H "Authorization: Bearer $TOKEN" -X GET "${ARTIFACTORY_URL}/artifactory/api/system/ping"
# check token: https://jwt.io/#encoded-jwt
```

### get artifact from artifactory
```sh
URL="https://artifactory.sbbgroup.net/artifactory/management-snapshots/com/ad/cicd/jenkins/jenkins-labeling-6b999cadc054-SNAPSHOT-jenkins.zip"
OUTPUT_FILE=`echo $URL | awk -F '/' '{print $(NF)}'`
rm $OUTPUT_FILE
curl -u $ARTIFACTORY_USER:$ARTIFACTORY_PASS -X GET  $URL -o $OUTPUT_FILE
ls -la $OUTPUT_FILE

mv $OUTPUT_FILE $OUTPUT_FILE-original
```

### upload to artifactory
```sh
URL="https://artifactory.sbbgroup.net/artifactory/management-snapshots/com/ad/cicd/jenkins/jenkins-labeling-6b999cadc054-SNAPSHOT-jenkins.zip"
OUTPUT_FILE=`echo $URL | awk -F '/' '{print $(NF)}'`
UPLOAD_FILE="jenkins-labeling-6b999cadc054-SNAPSHOT-jenkins.zip"
curl -u $ARTIFACTORY_USER:$ARTIFACTORY_PASS -X PUT  $URL --data-binary @${UPLOAD_FILE}

# curl -v --user username:password -X PUT urlGoesHere --data-binary fileToBeDeployed
```

### upload docker image to artifactory
```sh
DOCKER_USER=tech-user
DOCKER_TOKEN=shyqWHDzXMwtQ....
DOCKER_URL=artifactory.ubsgroup.com

docker login -u $DOCKER_USER -p $DOCKER_TOKEN $DOCKER_URL
DOCKER_IMAGE=$DOCKER_URL/project-docker/portal-e2e:2.0.0
docker push $DOCKER_IMAGE
```

## F.A.Q 
### error from endpoint
```
{
  "errors" : [ {
    "status" : 403,
    "message" : "The user: 'tu-datacenter' is not permitted to deploy ... 
  } ]
}
```
check user's permission 
