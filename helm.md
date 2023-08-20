# [helm](https://docs.helm.sh/)
package manager for Kubernetes  
( similar to pip for python, similar to apt to debian )

## links
* [helm documentation](https://helm.sh/docs/)
* [helm quick start](https://helm.sh/docs/intro/quickstart/)
* [interactive course helm](https://www.katacoda.com/aptem/scenarios/helm)

## helm charts
* [helm charts](https://artifacthub.io/)
* [operator hub - solutions for installing](https://operatorhub.io/)

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

# add new repo
helm repo add my_local_repo https://charts.bitnami.com/bitnami
# helm install --set replica.replicaCount=1 my_local_repo/redis

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

# some examples 
helm install bitname/postgresql
helm install oci://registry-1.docker.io/bitnamicharts/postgresql
helm install my_own_postgresql bitname/postgresql
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


### issue with postgresql, issue with mapping PV ebs.csi.aws.com
```text
"message": "running PreBind plugin \"VolumeBinding\": binding volumes: timed out waiting for the condition",
```
create local storage class instead of mapping to external ( EBS )
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: Immediate
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: local-storage
spec:
  capacity:
    storage: 8Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Delete
  storageClassName: local-storage
  local:
    path: /tmp
  nodeAffinity:
    required:
      nodeSelectorTerms:
        - matchExpressions:
            - key: kubernetes.io/hostname
              operator: In
              values:
                - <NODE_INSTANCE_IP>
```
```sh
helm repo add $HELM_BITNAMI_REPO https://charts.bitnami.com/bitnami
helm install $K8S_SERVICE_POSTGRESQL $HELM_BITNAMI_REPO/postgresql --set global.storageClass=local-storage
```

