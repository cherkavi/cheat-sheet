# [helm](https://docs.helm.sh/)

## links
* [helm documentation](https://helm.sh/docs/)
* [interactive course helm](https://www.katacoda.com/aptem/scenarios/helm)

## Architecture
![main components](https://i.postimg.cc/gkBhFQHG/helm-architecture.png)

## [installation](https://helm.sh/docs/intro/install/)
```sh
sudo snap install helm --classic
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | bash
```

## de-installation
```sh
helm reset
```

## initialization
```sh
helm init
# sync latest available packages
helm repo update
```

## useful variables
```sh
# the location of Helm's configuration
echo $HELM_HOME
# the host and port that Tiller is listening on
echo $TILLER_HOST
# the path to the helm command on your system
echo $HELM_BIN
# the full path to this plugin (not shown above, but we'll see it in a moment).
echo $HELM_PLUGIN_DIR
```

## analyze local package
```sh
helm inspect { folder }
helm lint { folder }
```

## search remote package
```sh
helm search 
helm describe {full name of the package}
```

## information about remote package
```sh
helm info {name of resource}
helm status {name of resource}
```

## create package locally
```sh
helm create 
```
![folder structure](https://i.postimg.cc/d1kXZrL7/helm-sceleton.png)

### create package with local templates
```sh
ls -la ~/.helm/starters/
```

## install package
```sh
helm install { full name of the package }
helm install --name {my name for new package} { full name of the package }
helm install --name {my name for new package} --namespace {namespace} -f values.yml --debug --dry-run { full name of the package }
```

## install aws plugin
```sh
helm plugin install https://github.com/hypnoglow/helm-s3.git
```

## list of installed packages
```sh
helm list
helm list --all
helm ls
```

## package upgrade
local package
```sh
helm upgrade  {deployment/svc/rs/rc name} . --set replicas=2,maria.db.password="new password"
```
package by name
```sh
helm upgrade {name of package} {folder with helm scripts} --set replicas=2
```

check upgrade
```sh
helm history
helm rollback {name of package} {revision of history}
```

## remove packageHelm
```sh
helm delete --purge {name of package}
```


## trouble shooting
### issue with 'helm list'
```
E1209 22:25:57.285192    5149 portforward.go:331] an error occurred forwarding 40679 -> 44134: error forwarding port 44134 to pod de4963c7380948763c96bdda35e44ad8299477b41b5c4958f0902eb821565b19, uid : unable to do port forwarding: socat not found.
Error: transport is closing
```
solution
```sh
sudo apt install socat
```

### incompatible version of client and server
```
Error: incompatible versions client[v2.12.3] server[v2.11.0]
```
solution
```sh
helm init --upgrade
kubectl get pods --namespace kube-system # waiting for start Tiller
helm version
```
