### minishift documentation, help url
> https://docs.openshift.org/latest/minishift/using/index.html

### install client
download appropriate release 
```
https://api.github.com/repos/openshift/origin/releases/latest
```
retrieve "browser_download_url", example of link for downloading ( from previous link )
```
https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz
tar -xvf openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz
mv openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit /home/soft/openshift-tool
export PATH=/home/soft/openshift-tool:$PATH
```

### login into local minishift
```
oc login --username=admin --password=admin
echo "my_password" | oc login -u my_user
oc login -u developer -p developer
oc login {url}
```
check login
```
oc whoami
oc whoami -t
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

### create token for MapR
```bash
maprlogin password -user {mapruser}
# ticket-file will be created
```
check expiration date
```
maprlogin print -ticketfile /tmp/maprticket_1000 # or another filename
```

using file from previous command
```bash
cat /tmp/maprticket_1000 
#oc create secret generic {name of secret/token} --from-file=/tmp/maprticket_1000 -n {project name}
oc create secret generic {name of secret/token} --from-file=CONTAINER_TICKET=/tmp/maprticket_1000 -n {project name}
```
or from content of file from previous command
```bash
oc create secret generic {name of secret/token} --from-literal=CONTAINER_TICKET='dp.prod.ubs qEnHLE7UaW81NJaDehSH4HX+m9kcSg1UC5AzLO8HJTjhfJKrQWdHd82Aj0swwb3AsxLg==' -n {project name}
```
check creation
```bash
oc get secrets
```

### describe information about cluster
```
oc describe {[object type:](https://docs.openshift.com/enterprise/3.0/cli_reference/basic_cli_operations.html#object-types)}
```
* buildconfigs
* services
* routes
* ...

### show namespace, all applications, url to service, status of all services
```
oc status
```

### show route to service, show url to application
```
oc get routes {app name / service name}
```

## get all information about current project, show all resources
```
oc get all
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
```
oc project
oc policy add-role-to-user admin cherkavi
# oc policy remove-role-from-user admin cherkavi
oc get rolebindings
```

### create project
```
oc get projects
oc new-project {project name}
oc describe project {project name}
```

### print current project
```
oc project
```

### project select, select project
```
oc project {project name}
```

### create resource ( pod, job, volume ... )
```
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
      restartPolicy: Never
  backoffLimit: 4
```

### set resource limits
```
oc set resources dc/{app-name} --limits=cpu=400m,memory=512Mi --requests=cpu=200m,memory=256Mi
oc autoscale dc/{app-name} --min 1 --max 5 --cpu-percent=40
```

### connect to existing pod, execute command on remote pod, oc exec
```
oc get pods
oc rsh {name of pod}
# example of executing program on pod: kafka-test-app
oc exec kafka-test-app "/usr/bin/java"
```

### [create app](https://access.redhat.com/documentation/en-us/openshift_enterprise/3.0/html/developer_guide/dev-guide-new-app)

#### new app with "default" container 
```
oc new-app {/local/folder/to_source}
```

#### new app with "default" container from GIT
```
oc new-app https://github.com/openshift/ruby-ex.git
```

#### new app with "specific" (centos/ruby-22-centos7) docker container from GIT
```
oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-ex.git
```

#### new app with "specific" (centos/ruby-22-centos7) docker container from GIT with specific sub-folder and name
```
oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-ex.git --context-dir=sub-project --name myruby
```

#### import specific image
```
oc import-image jenkins:v3.7 --from='registry.access.redhat.com/openshift3/jenkins-2-rhel7:v3.7' --confirm -n openshift
```

### log from 
```
oc logs pod/{name of pod}
oc logs --follow bc/{name of app}
```

### describe resource, information about resource
```
oc describe job {job name}
oc describe pod {pod name}
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

### information about cluster
```
kubectl cluster-info
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

### pod example
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
  volumes:
  - name: maprvolume-source
    persistentVolumeClaim:
      claimName: pvc-scenario-input-prod
  - name: maprvolume-destination
    persistentVolumeClaim:
      claimName: pvc-scenario-output-prod
  restartPolicy: Never
  backoffLimit: 4
```
### Persistent Volume with Persistent Volume Claim example
For MapR cluster, be aware about
MapR ticket-file ----<>Secret-----<>PV------<>PVC
```
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
