### minishift documentation, help url
> https://docs.openshift.org/latest/minishift/using/index.html

### login into local minishift
> oc login --username=admin --password=admin
> oc login -u developer -p developer
> oc login {url}

### describe information about cluster
> oc describe {[object type:](https://docs.openshift.com/enterprise/3.0/cli_reference/basic_cli_operations.html#object-types)}
- buildconfigs
- services
- routes
...

### show namespace, all applications, url to service, status of all services
> oc status

## get all information about current project, show all resources
> oc get all

### create project
> oc new-project {project name}

### print current project
> oc project

### project select, select project
> oc project {project name}

### create app
#### new app with "default" container from GIT
> oc new-app https://github.com/openshift/ruby-ex.git

#### new app with "specific" (centos/ruby-22-centos7) docker container from GIT
> oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-ex.git

#### new app with "specific" (centos/ruby-22-centos7) docker container from GIT with specific sub-folder and name
> oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-ex.git --context-dir=sub-project --name myruby

### log from 
> oc logs --follow bc/{name of app}

### Grant permission to be able to access OpenShift REST API and discover services.
> oc policy add-role-to-user view -n {name of application/namespace} -z default

### information about current configuration
> oc config view

### check accessible applications, ulr to application, application path
> oc describe routes
Requested Host:

### delete/remove information about some entities into project
> oc delete {type} {type name}
- buildconfigs
- services
- routes
...

### expose services
if your service looks like svc/web - 172.30.20.243:8080
instead of external link like: http://gateway-myproject.192.168.42.43.nip.io to pod port 8080 (svc/gateway), then you can "expose" it for external world:
> svn expose services/{app name}
> svn expose service/{app name}
> svn expose svc/{app name}



### current ip address
> minishift ip

### open web console
> minishift console


## Kubernetes

### print all context
kubectl config get-contexts

### pring current context
kubectl config current-context

### information about cluster
kubectl cluster-info

### api version
kubectl api-versions



--> Success
    Build scheduled, use 'oc logs -f bc/web' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose svc/web' 
    Run 'oc status' to view your app.