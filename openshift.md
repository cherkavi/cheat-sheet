### minishift documentation, help url
> https://docs.openshift.org/latest/minishift/using/index.html

### login into local minishift
> oc login --username=admin --password=admin
> oc login -u developer -p developer
> oc login {url}

### describe information about cluster
> oc describe {resource:}
- buildconfigs
- services
- routes
...

### show namespace, all applications, url to service
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
> oc new-app centos/ruby-22-centos7~https://github.com/openshift/ruby-ex.git

### Grant permission to be able to access OpenShift REST API and discover services.
> oc policy add-role-to-user view -n {name of application/namespace} -z default

### information about current configuration
> oc config view

### check accessible applications, ulr to application, application path
> oc describe routes
Requested Host:

### delete/remove information about some entities into project
> oc delete {type} {imagestream name}
- buildconfigs
- services
- routes
...


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

