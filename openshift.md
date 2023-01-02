# OpenShift 

## links collections
* [minishift documentation, help url](https://docs.openshift.org/latest/minishift/using/index.html)  
* [CLI commands oc commands](https://docs.openshift.com/container-platform/4.1/cli_reference/developer-cli-commands.html)  
* [tekton pipeline examples](https://github.com/dalelane/app-connect-tekton-pipeline)
* [openshift cicd](https://docs.openshift.com/container-platform/4.11/cicd/pipelines/understanding-openshift-pipelines.html)

## Init
### install client 
[oc cli installation](https://docs.openshift.com/container-platform/4.6/cli_reference/openshift_cli/getting-started-cli.html)
debian
```sh
sudo apt install oc
```
or [download appropriate release](https://api.github.com/repos/openshift/origin/releases/latest)  
or [openshift downloads](https://access.redhat.com/downloads)
retrieve "browser_download_url", example of link for downloading ( from previous link )
```
https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz
tar -xvf openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz
mv openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit /home/soft/openshift-tool
export PATH=/home/soft/openshift-tool:$PATH
```
check oc client version oc
```sh
oc version -v8 2>&1 | grep "User-Agent" | awk '{print $6}
```
[odo cli](https://docs.openshift.com/container-platform/latest/cli_reference/opm/cli-opm-install.html)  
[tkn cli](https://docs.openshift.com/container-platform/latest/cli_reference/tkn_cli/installing-tkn.html)  

### completion
```bash
source <(oc completion bash)
```
### trace logging communication, verbose output
```bash
rm -rf ~/.kube/cache
oc get pods -v=6
oc get pods -v=7
oc get pods -v=8
```

## REST api
### print collaboration, output rest api call, print api calls
```sh
oc whoami -v=8
```
### example of rest api collaboration, rest call
```bash
TOKEN=$(oc whoami -t)
ENDPOINT=$(oc status | head --lines=1 | awk '{print $6}')
NAMESPACE=$(oc status | head --lines=1 | awk '{print $3}')
echo $TOKEN
echo $ENDPOINT
echo $NAMESPACE
echo $NAME

curl -k -H "Authorization: Bearer $TOKEN" -H 'Accept: application/json' $ENDPOINT/api/v1/pods
curl -k -H "Authorization: Bearer $TOKEN" -H 'Accept: application/json' $ENDPOINT/api/v1/namespaces/$NAMESPACE/pods
# watch on changes
curl -k -H "Authorization: Bearer $TOKEN" -H 'Accept: application/json' $ENDPOINT/api/v1/watch/namespaces/$NAMESPACE/pods
```

## Login
### login into openshift
```sh
oc login --username=admin --password=admin
echo "my_password" | oc login -u my_user
oc login -u developer -p developer
oc login {url}
```
check login
```sh
oc whoami
oc whoami -t
# or 
oc status | grep "on server"
```
### login into openshift using token
https://oauth-openshift.stg.zxxp.zur/oauth/token/display 
```sh
 oc login --token=sha256~xxxxxxxxxxxxx --server=https://api.stg.zxxp.zur:6443
```

### switch contex, use another cluster
~/.kube/config
```
apiVersion: v1
clusters:
- cluster:
    insecure-skip-tls-verify: true
    server: https://localhost:6440
  name: docker-for-desktop-cluster   
- cluster:
    insecure-skip-tls-verify: true
    server: https://openshift-master-sim.myprovider.org:8443
  name: openshift-master-sim-myprovider-org:8443
```
```
kubectl config use-context kubernetes-admin@docker-for-desktop-cluster
```

## explain yaml schema
```
oc explain pods
oc explain pods --recursive
oc explain pods --recursive --api-version=autoscaling/v2beta1
```
## get in yaml, get source of resource, describe yaml
```
oc get -o yaml  pod {name of the pod}
oc get -o json  pod {name of the pod}

oc get -o json  pod {name of the pod} --namespace one --namespace two --namespace three
```

## secrets 
### create token for MapR
```bash
maprlogin password -user {mapruser}
# ticket-file will be created
```
### check expiration date
```
maprlogin print -ticketfile /tmp/maprticket_1000 # or another filename
```

### create secret from file
```bash
cat /tmp/maprticket_1000 
# create secret from file ( default name )
oc create secret generic {name of secret/token} --from-file=/tmp/maprticket_1000 -n {project name}
# create secret from file with specifying the name - CONTAINER_TICKET ( oc describe {name of secret} )
oc create secret generic {name of secret/token} --from-file=CONTAINER_TICKET=/tmp/maprticket_1000 -n {project name}
```
### read secret get secret value
```sh
oc get secret $TICKET_NAME -o yaml | yq .data | awk '{print $2}' | base64 --decode
```

### automation for creating tickets in diff namespaces
```sh
function openshift-replace-maprticket(){
    MAPR_TICKET_PATH="${1}"
    if [[ $MAPR_TICKET_PATH == "" ]]; then
        echo " first parameter should be filepath to MapR ticket PROD ! "
        return 1
    fi
    if [ ! -f $MAPR_TICKET_PATH ]; then
        echo "can't access file: ${MAPR_TICKET_PATH}"
        return 2
    fi
    oc login -u $TECH_USER -p $TECH_PASSWORD $OPEN_SHIFT_URL
    PROJECTS=("portal-pre-prod" "portal-production")
    SECRET_NAME="mapr-ticket"
    
    for OC_PROJECT in "${PROJECTS[@]}"
    do 
        echo $OC_PROJECT
        oc project $OC_PROJECT
        oc delete secret $SECRET_NAME
        oc create secret generic $SECRET_NAME --from-file=CONTAINER_TICKET=$MAPR_TICKET_PATH -n $OC_PROJECT
        oc get secret $SECRET_NAME -n $OC_PROJECT
    done
}
```
or from content of file from previous command  
```bash
oc create secret generic {name of secret/token} --from-literal=CONTAINER_TICKET='dp.prod.ubs qEnHLE7UaW81NJaDehSH4HX+m9kcSg1UC5AzLO8HJTjhfJKrQWdHd82Aj0swwb3AsxLg==' -n {project name}
```

### create secret values 
login password secret
```
SECRET_NAME=my-secret
KEY1=user
VALUE1=cherkavi
KEY2=password
VALUE2=my-secret-password
oc create secret generic $SECRET_NAME --from-literal=$KEY1=$VALUE1 --from-literal=$KEY2=$VALUE2
```
or even mix of them
```
oc create secret generic $SECRET_NAME --from-file=ssh-privatekey= /.ssh/id_rsa --from-literal=$KEY1=$VALUE1
```

check creation  
```bash
oc get secrets
oc get secret metadata-api-token -o yaml | yq .data.METADATA_API_TOKEN | base64 --decode
```
secret mapping example, map secret  
```json

   ...
   volumeMounts:
          - name: mapr-ticket
            mountPath: "/path/inside/container"
            readOnly: true
...            
 volumes:
        - name: mapr-ticket
          secret:
            secretName: my-ticket
```

### information about cluster
```
kubectl cluster-info
```

### describe information about cluster
```
oc describe {[object type:](https://docs.openshift.com/enterprise/3.0/cli_reference/basic_cli_operations.html#object-types)}
```
* buildconfigs
* services
* routes
* ...

### take a look into all events, notification about changes
```sh
# follow events
oc get --watch events
# print events and sort them out by time
oc get events --sort-by='.lastTimestamp' | grep " Warning "
```

### show namespace, all applications, url to service, status of all services
```
oc status
```

### show route to service, show url to application
```sh
oc get routes {app name / service name}
oc get route -demo hello-world-http -o jsonpath='{.spec.host}'
```
#### route migration
```sh
FILE_NAME=route-data-api-mdf4download-service.yaml
echo "vim $FILE_NAME" | clipboard
yq 'del(.metadata.managedFields,.status,.metadata.uid,.metadata.resourceVersion,.metadata.creationTimestamp,.metadata.labels."template.openshift.io/template-instance-owner"),(.metadata.namespace="my_namespace")' $FILE_NAME 
```

## get all information about current project, show all resources
```sh
oc get all
oc get deployment,pod,service,route,dc,pvc,secret -l deployment_name=name-of-my-deployment
oc get route/name-of-route --output json
```

### restart pod
```sh
oc rollout latest "deploy-config-example"
```
## service 
### get services
```sh
oc get services
```

### service curl inside OCP
```sh
curl http://${SERVICE_NAME}:${SERVICE_PORT}/data-api/v1/health/
```

### service migration
```
FILE_NAME=service-data-portal.yaml
oc get service/my_service --output yaml > $FILE_NAME
echo "vim $FILE_NAME" | clipboard
yq 'del(.metadata.managedFields,.status,.metadata.uid,.metadata.resourceVersion,.metadata.creationTimestamp,.spec.clusterIP,.spec.clusterIPs),(.metadata.namespace="my_new_namespace")' $FILE_NAME | clipboard
```

## print all accounts
```
oc get serviceaccounts
```

## print all roles, check assigned roles, get users, list of users
```
oc get rolebindings
```

## add role to current project, assign role to project, remove role from user
```sh
oc project
oc policy add-role-to-user admin cherkavi
# oc policy remove-role-from-user admin cherkavi
oc get rolebindings
```

### create project
```sh
oc get projects
oc new-project {project name}
oc describe project {project name}
```

### images internal registry get images
```sh
oc get images
oc get images.image.openshift.io
```

### image import docker import to internal registry 
```sh
IMAGE_OCP=image-registry.openshift-registry.svc:5000/portal-test-env/openjdk-8-slim-enhanced:ver1
IMAGE_EXTERNAL=nexus-shared.com/repository/uploadimages/openjdk-8-slim-enhanced:202110
oc import-image $IMAGE_OCP --reference-policy='local' --from=$IMAGE_EXTERNAL --confirm
```
```sh
oc import-image approved-apache --from=bitnami/apache:2.4 --confirm
oc import-image my-python --from=my-external.com/tdonohue/python-hello-world:latest --confirm

# if you have credential restrictions
# oc create secret docker-registry my-mars-secret --docker-server=registry.marsrover.space --docker-username="login@example.com" --docker-password=thepasswordishere
```
!!! in case of any errors in process creation, pay attention to output of pods/....-build

### build configs for images
```sh
oc get bc
oc describe bc/user-portal-dockerbuild 
```

### tag image stream tag image
```sh
oc tag my-external.com/tdonohue/python-hello-world:latest my-python:latest
# Tag a specific image
oc tag openshift/ruby@sha256:6b646fa6bf5e5e4c7fa41056c27910e679c03ebe7f93e361e6515a9da7e258cc yourproject/ruby:tip

# Tag an external container image
oc tag --source=docker openshift/origin-control-plane:latest yourproject/ruby:tip

# check tag
oc get is
```

## pod image sources pod docker image sources
just an example from real cluster
```
             │       ┌─────────┐
 LOCAL       │       │ Nexus   │
     ┌───────┼──1────► docker  ├────┐───────┐
     │       │       │ storage │    │       │
     │       │       └─────────┘    3       4
     │       │                      │       │
┌────┴─────┐ ├──────────────────────┼───────┼─
│  docker  │ │    OpenShift         │       │
└────┬─────┘ │                  ┌───▼──┐    │
     │       │ ┌─────────┐      │Image │    │
     │       │ │OpenShift│      │Stream│    │
     └──2────┼─►registry │      └─┬────┘    │
             │ └────┬────┘        5         │
             │      │        ┌────▼─┐       │
             │      └────6───► POD  ◄───────┘
                             └──────┘
```
1. docker push <nexus>
2. docker push <openshift>
3. import-image

### image stream build
in case of "no log output" during the input stream creation - check "-build" image not in "terminating" state
	
	
### print current project
```sh
oc project
```

### project select, select project
```sh
oc project {project name}
```

### create resource ( pod, job, volume ... )
```sh
oc create -f {description file}
# oc replace -f {description file}
```
example of job
```
apiVersion: batch/v1
kind: Job
metadata:
  name: scenario-description
spec:
  nodeSelector:         
    composer: true
  template:         
    spec:
      containers:
      - name: scenario-description
        image: cc-artifactory.myserver.net/add-docker/scenario_description:0.23.3
        command: ["python", "-c", "'import scenario_description'"]
        env:
          - name: MAPR_TICKETFILE_LOCATION
            value: "/tmp/maprticket_202208"        
          # set environment variable from metadata
          - name: PROJECT
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace            
            - name: MAPR_TZ
              valueFrom:
                configMapKeyRef:
                  name: environmental-vars
                  key: MAPR_TZ
            - name: POSTGRES_USER
              valueFrom:
                secretKeyRef:
                  name: postgresqlservice
                  key: database-user                
      restartPolicy: Never
  backoffLimit: 4
```
![image](https://user-images.githubusercontent.com/8113355/178698083-47b2e115-f802-4aaa-a960-a6d3f0e10707.png)
1. secret to env
```yaml
          env:
            - name: POSTGRESUSER
              valueFrom:
                secretKeyRef:
                  name: postgresql-service
                  key: database-user

```
2. configmap to env
```yaml
          env:
            - name: MAPRCLUSTER
              valueFrom:
                configMapKeyRef:
                  name: env-vars
                  key: MAPR_CLUSTER
```
```yaml
    envFrom:
    - configMapRef:
        name: session-config-map
    - secretRef:
        name: session-secret
```
3. secret to file
```yaml
    spec:
      volumes:
        - name: mapr-ticket
          secret:
            secretName: mapr-ticket
            defaultMode: 420
...
          volumeMounts:
            - name: mapr-ticket
              readOnly: true
              mountPath: /users-folder/maprticket

```
4. configmap to file
```yaml
    spec:
      volumes:
        - name: logging-config-volume
          configMap:
            name: log4j2-config
            defaultMode: 420
...
          volumeMounts:
            - name: logging-config-volume
              mountPath: /usr/src/config
```

### set resource limits
```
oc set resources dc/{app-name} --limits=cpu=400m,memory=512Mi --requests=cpu=200m,memory=256Mi
oc autoscale dc/{app-name} --min 1 --max 5 --cpu-percent=40
```

### connect to existing pod in debug mode, debug pod
```bash
# check policy
# oc adm policy add-scc-to-user mapr-apps-scc system:serviceaccount:${PROJECT_NAME}:default
# oc adm policy add-role-to-user admin ${USER_NAME} -n ${PROJECT_NAME}

oc debug pods/{name of the pod}
oc debug dc/my-dc-config --as-root --namespace my-project

# start container after fail
oc rollout latest {dc name}
# stop container after fail
oc rollback latest {dc name}
```

### connect to existing pod, execute command on remote pod, oc exec
```
oc get pods --field-selector=status.phase=Running
oc rsh <name of pod>
oc rsh -c <container name> pod/<pod name>

# connect to container inside the pod with multi container
POD_NAME=data-portal-67-dx
CONTAINER_NAME=data-portal-apache
oc exec -it -p $POD_NAME -c $CONTAINER_NAME /bin/bash
# or 
oc exec -it $POD_NAME -c $CONTAINER_NAME /bin/bash
```

### execute command in pod command
```
# example of executing program on pod: kafka-test-app
oc exec kafka-test-app "/usr/bin/java"
```

### get environment variables
```sh
oc set env pod/$POD_DATA_API --list
```

### copy file 
```sh
# copy file from pod
oc cp <local_path> <pod_name>:/<path> -c <container_name>  
oc cp api-server-256-txa8n:usr/src/cert/keystore_server /my/local/path
# for OCP4 we should NOT to use leading slash like /usr/src.... 

# copy files from POD to locally 
oc rsync /my/local/folder/ test01-mz2rf:/opt/app-root/src/

# copy file to pod
oc cp <pod_name>:/<path>  -c <container_name><local_path>  
```

### forward port forwarding
```bash
oc port-forward <pod-name> <ext-port>:<int-port>
```
```sh
function oc-port-forwarding(){
    if [[ $# != 3 ]]
    then
        echo "port forwarding for remote pods with arguments:"
        echo "1. project-name, like 'portal-stg-8' "
        echo "2. pod part of the name, like 'collector'"
        echo "3. port number like 5005"
        return 1
    fi

	oc login -u $USER_DATA_API_USER -p $USER_DATA_API_PASSWORD $OPEN_SHIFT_URL
	oc project $1
    POD_NAME=$(oc get pods | grep Running | grep $2 | awk '{print $1}')
    echo $POD_NAME
    oc port-forward $POD_NAME $3:$3
}
```

### [create app](https://access.redhat.com/documentation/en-us/openshift_enterprise/3.0/html/developer_guide/dev-guide-new-app)

#### new app with "default" container 
```sh
oc new-app {/local/folder/to_source}
```

#### new app with "default" container from GIT
```sh
oc new-app https://github.com/openshift/ruby-ex.git
```

#### new app with "specific" (centos/ruby-22-centos7) docker container from GIT
```sh
oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-ex.git
```

#### new app with "specific" (centos/ruby-22-centos7) docker container from GIT with specific sub-folder and name
```sh
oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-ex.git --context-dir=sub-project --name myruby
```
#### openshift new application create and check
```sh
OC_APP_NAME=python-test
OC_PROJECT_NAME=my-project
OC_ROOT=app.vantage.ubs

# cleanup before creation
oc delete route $OC_APP_NAME
oc delete service $OC_APP_NAME
oc delete deployment $OC_APP_NAME
oc delete buildconfigs.build.openshift.io $OC_APP_NAME

# create new oc application from source code
oc new-app https://github.com/cherkavi/python-deployment.git#main --name=$OC_APP_NAME

# check deployment
oc get service $OC_APP_NAME -o yaml
oc get deployment $OC_APP_NAME -o yaml
oc get buildconfigs.build.openshift.io $OC_APP_NAME -o yaml

# create route
oc create route edge $OC_APP_NAME --service=$OC_APP_NAME
# check route: tls.termination: edge
oc get route $OC_APP_NAME -o yaml

curl -X GET https://${OC_APP_NAME}-${OC_PROJECT_NAME}.${OC_ROOT}/
```

#### import specific image
```
oc import-image jenkins:v3.7 --from='registry.access.redhat.com/openshift3/jenkins-2-rhel7:v3.7' --confirm -n openshift
```

### log from 
```
oc logs pod/{name of pod}
oc logs -c <container> pods/<pod-name>
oc logs --follow bc/{name of app}
```

### describe resource, information about resource
```sh
oc describe job {job name}
oc describe pod {pod name}
```

### edit resource
```sh
export EDITOR=vim
oc edit pv pv-my-own-volume-1
```
or 
```sh
oc patch pv/pv-my-own-volume-1 --type json -p '[{ "op": "remove", "path": "/spec/claimRef" }]'
```

### debug pod
```sh
oc debug deploymentconfig/$OC_DEPL_CONFIG -c $OC_CONTAINER_NAME --namespace $OC_NAMESPACE
```
[container error pod error](https://docs.openshift.com/container-platform/4.5/support/troubleshooting/investigating-pod-issues.html)


### config map
```sh
# list of config maps
oc get configmap

# describe one of the config map 
oc get configmaps "httpd-config" -o yaml
oc describe configmap data-api-config
oc describe configmap gatekeeper-config

```

### Grant permission to be able to access OpenShift REST API and discover services.
```
oc policy add-role-to-user view -n {name of application/namespace} -z default
```

### information about current configuration
```
oc config view
```
the same as
```
cat ~/.kube/config/config
```

### check accessible applications, ulr to application, application path
```
oc describe routes
```
Requested Host:

### delete/remove information about some entities into project
```
oc delete {type} {type name}
```
* buildconfigs
* services
* routes
* ...

#### Isio external service exposing
```sh
oc get svc istio-ingressgateway -n istio-system
```
### expose services
if your service looks like svc/web - 172.30.20.243:8080
instead of external link like: http://gateway-myproject.192.168.42.43.nip.io to pod port 8080 (svc/gateway), then you can "expose" it for external world:
* svn expose services/{app name}
* svn expose service/{app name}
* svn expose svc/{app name}

### Liveness and readiness probes
```
# set readiness/liveness
oc set probe dc/{app-name} --liveness --readiness --get-url=http://:8080/health
# remove readiness/liveness
oc set probe dc/{app-name} --remove --liveness --readiness --get-url=http://:8080/health
# oc set probe dc/{app-name} --remove --liveness --readiness --get-url=http://:8080/health --initial-delay-seconds=30
 
# Set a readiness probe to try to open a TCP socket on 3306
oc set probe rc/mysql --readiness --open-tcp=3306
```
*Readiness probe* will stop after first positive check  
*Liveness probe* will be executed again and again (period) during container lifetime  
![image](https://user-images.githubusercontent.com/8113355/207673876-08e257a4-cc10-4549-a36d-98dc25b55f46.png)

### current ip address
```
minishift ip
```

### open web console
```
minishift console
```

## Kubernetes

### print all context
```
kubectl config get-contexts
```

### pring current context
```
kubectl config current-context
```


### api version
```
kubectl api-versions
```

--> Success
    Build scheduled, use 'oc logs -f bc/web' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose svc/web' 
    Run 'oc status' to view your app.

### job example
!!! openshift job starts only command - job will skip entrypoint 
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: test-job-traceroute
spec:
  nodeSelector:         
    composer: true
  template:         
    spec:
      containers:
      - name: busybox
        image: busybox
        command: ["traceroute", "cc-art.group.net"]
          
      restartPolicy: Never
  backoffLimit: 4		
```
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: scenario-description
spec:
  template:         
    spec:
      containers:
      - name: scenario-description
        image: scenario_description:0.2.3
        command: ["python", "-c", "'import scenario_description'"]
      restartPolicy: Never
```

### pod example simple pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test01
spec:
  containers:
  - name: test01
    image: busybox
    command: ["sleep", "36000"]
  restartPolicy: Never
  backoffLimit: 4
```

### pod sidecar
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test01
spec:
  containers:
  - name: test01
    image: busybox
    command: ["sleep", "36000"]
  - name: test02
    image: busybox
    command: ["sleep", "36000"]
  restartPolicy: Never
  backoffLimit: 4
```


### pod with mapping
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: connect-to-me
spec:
  containers:
  - name: just-a-example
    image: busybox
    command: ["sleep", "36000"]
    volumeMounts:
    - mountPath: /source
      name: maprvolume-source
    - mountPath: /destination
      name: maprvolume-destination
    - name: httpd-config-volume
      mountPath: /usr/local/apache2/conf/httpd.conf      
  volumes:
  - name: maprvolume-source
    persistentVolumeClaim:
      claimName: pvc-scenario-input-prod
  - name: maprvolume-destination
    persistentVolumeClaim:
      claimName: pvc-scenario-output-prod
  - name: httpd-config-volume
    configMap:
      name: httpd-config
      defaultMode: 420      
  restartPolicy: Never
  backoffLimit: 4
```
### Persistent Volume with Persistent Volume Claim example
For MapR cluster, be aware about
MapR ticket-file ----<>Secret-----<>PV------<>PVC

### pv mapr
```yaml
kind: PersistentVolume
apiVersion: v1
metadata:
  name: pv-workloads-staging-01
spec:
  capacity:
    storage: 50Gi
  csi:
    driver: com.mapr.csi-kdf
    volumeHandle: pv-workloads-staging-01
    volumeAttributes:
      cldbHosts: >-
        dpmtjp0001.swiss.com dpmtjp0002.swiss.com
        dpmtjp0003.swiss.com dpmtjp0004.swiss.com
      cluster: dp.stg.swiss
      platinum: 'false'
      securityType: secure
      volumePath: /data/reprocessed/sensor
    nodePublishSecretRef:
      name: hil-supplier-01
      namespace: workloads-staging
  accessModes:
    - ReadWriteMany
  claimRef:
    kind: PersistentVolumeClaim
    namespace: workloads-staging
    name: pvc-supplier-01
  persistentVolumeReclaimPolicy: Retain
  volumeMode: Filesystem
status:
  phase: Bound
```
```yaml
kind: PersistentVolume
apiVersion: v1
metadata:
  name: pv-mapr-tmp
spec:
  capacity:
    storage: 10Gi
  csi:
    driver: com.mapr.csi-kdf
    volumeHandle: pv-mapr-tmp
    volumeAttributes:
      cldbHosts: >-
        esp000004.swiss.org esp000007.swiss.org
        esp000009.swiss.org esp000010.swiss.org
      cluster: prod.zurich
      securityType: secure
      volumePath: /tmp/
    nodePublishSecretRef:
      name: mapr-secret
      namespace: pre-prod
  accessModes:
    - ReadWriteMany
  claimRef:
    kind: PersistentVolumeClaim
    namespace: pre-prod
    name: pvc-mapr-tmp
    apiVersion: v1
  persistentVolumeReclaimPolicy: Delete
  volumeMode: Filesystem
status:
  phase: Bound
```
if you are going to edit/change PV you should:
1. remove PV
2. remove PVC
3. remove all Workloads, that are using it ( decrease amount of PODs in running config )

### create secret token if it not exist
creating secret 
* login into mapr
```bash
echo $CLUSTER_PASSWORD | maprlogin password -user $CLUSTER_USER
```
* check secret for existence
```bash
oc get secrets -n $OPENSHIFT_NAMESPACE
```
* re-create secret
```bash
# delete secret 
oc delete secret/volume-token-ground-truth
cat /tmp/maprticket_1000

# create secret from file
ticket_name="cluster-user--mapr-prd-ticket-1536064"
file_name=$ticket_name".txt"
project_name="tsa"
## copy file from cluster to local folder
scp -r cluster-user@jump.server:/full/path/to/$file_name .
oc create secret generic $ticket_name --from-file=$file_name -n $OPENSHIFT_NAMESPACE
oc create secret generic volume-token-ground-truth --from-file=CONTAINER_TICKET=/tmp/maprticket_1000 -n $OPENSHIFT_NAMESPACE
oc create secret generic volume-token-ground-truth --from-literal=CONTAINER_TICKET='dp.prod.zurich qEnHLE7UaW81NJaDehSH4HX+m9kcSg1UC5AzLO8HJTjhfJKrQWdHd82Aj0swwb3AsxLg==' -n $OPENSHIFT_NAMESPACE

```
* check created ticket
```bash
maprlogin print -ticketfile /tmp/maprticket_1000
oc describe secret volume-token-ground-truth
```

using secret   
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-scenario-extraction-input
  namespace: scenario-extraction
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  claimRef:
    namespace: scenario-extraction
    name: pvc-scenario-extraction-input
  flexVolume:
    driver: "mapr.com/maprfs"
    options:
      platinum: "false"
      cluster: "dp.prod.munich"
      cldbHosts: "dpmesp000004.gedp.org dpmesp000007.gedp.org dpmesp000010.gedp.org dpmesp000009.gedp.org"
      volumePath: "/tage/data/store/processed/ground-truth/"
      securityType: "secure"
      ticketSecretName: "volume-token-ground-truth"
      ticketSecretNamespace: "scenario-extraction"
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-scenario-extraction-input
  namespace: scenario-extraction
spec:
  accessModes:
    - ReadWriteOnce
  volumeName: pv-scenario-extraction-input
  resources:
    requests:
      storage: 1G
```

### service example
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-pod
spec:
  selector:
    matchLabels:
      run: my-flask
  replicas: 1
  template:
    metadata:
      labels:
        run: my-flask
    spec:
      containers:
      - name: flask-test
        image: docker-registry.zur.local:5000/test-flask:0.0.1
        command: ["sleep","3600"]
        ports:
        - containerPort: 5000
---
apiVersion: v1
kind: Service
metadata:
  name: flask-service
  labels:
    run: my-flask
spec:
  ports:
  - name: flask
    port: 5000
    protocol: TCP
  - name: apache
    port: 9090
    protocol: TCP
    targetPort: 80
  selector:
    run: my-flask
```

### Deployment config max parameters for starting pods with long startup time
```yaml
# rule:
# readiness_probe.initial_delay_seconds <=  stategy.rollingParams.timeoutSeconds

stategy
  rollingParams
    timeoutSeconds: 1500

readiness_probe:
  initial_delay_seconds: 600
```


## mounting types volum mounting
```json
  volumeMounts:
    - { mountPath: /tmp/maprticket,                                name: mapr-ticket, readonly: true }
    - { mountPath: /usr/src/classes/config/server,                 name: server-config-volume, readonly: false }
    - { mountPath: /mapr/prod.zurich/vantage/data/store/processed, name: processed, readonly: false }
    - { mountPath: /tmp/data-api,                                  name: cache-volume, readonly: false }
  volumes:
    - { type: secret,    name: mapr-ticket,           secretName: mapr-ticket }
    - { type: configMap, name: server-config-volume, config_map_name: server-config }
    - { type: other,     name: mapr-deploy-data-api}
    - { type: pvc,       name: processed,            pvc_name: pvc-mapr-processed-prod }
    - { type: emptyDir,  name: cache-volume }
```

## access commands permissions granting
### check permission
```sh
oc get scc
```
for mapr container you should see:
* adasng: false(["NET_ADMIN", "SETGID", "SETUID"])
* anyuid: true("SYS_CHROOT")
* mapr-ars-scc: false()
* privileged: true(["*"])

### add permissions
```sh
oc adm policy add-scc-to-user privileged -z default -n my-ocp-project
```

### add security context constraint
```
oc adm policy add-scc-to-user {name of policy} { name of project }
oc adm policy remove-scc-to-user {name of policy} { name of project }
```
