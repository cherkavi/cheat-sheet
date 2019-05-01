---
useful links
* [cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
* [docs task by section](https://kubernetes.io/docs/tasks/)
* [k8s examples](https://github.com/wardviaene/kubernetes-course)
* [playground](https://labs.play-with-k8s.com/)
* [playground](https://www.katacoda.com/courses/kubernetes/playground)

---
# Architecture
![architecture](https://i.postimg.cc/TwZs4CN0/k8s-architecture-overview.png)
![nodes with software](https://i.postimg.cc/QCHz6vqH/k8s-architecture.png)
![kubernetes](https://i.postimg.cc/CL1Z9Lnv/kubernetes.png)
---
# microk8s
## installation
* https://github.com/ubuntu/microk8s
* https://microk8s.io/

```
sudo snap install microk8s --classic
sudo snap install microk8s --classic --edge 
```
enable addons
```
microk8s.start
microk8s.enable dns dashboard
```
check installation
```
microk8s.inspect
```
check journals for services
```
journalctl -u snap.microk8s.daemon-docker
```
* snap.microk8s.daemon-apiserver
* snap.microk8s.daemon-controller-manager
* snap.microk8s.daemon-scheduler
* snap.microk8s.daemon-kubelet
* snap.microk8s.daemon-proxy
* snap.microk8s.daemon-docker
* snap.microk8s.daemon-etcd
---
# minikube
## installation
```
sudo snap install minikube
```

## installation
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl

export MINIKUBE_WANTUPDATENOTIFICATION=false
export MINIKUBE_WANTREPORTERRORPROMPT=false
export MINIKUBE_HOME=$HOME
export CHANGE_MINIKUBE_NONE_USER=true
mkdir $HOME/.kube || true
touch $HOME/.kube/config

export KUBECONFIG=$HOME/.kube/config
sudo -E ./minikube start --vm-driver=none

# wait that Minikube has created
for i in {1..150}; do # timeout for 5 minutes
   ./kubectl get po &> /dev/null
   if [ $? -ne 1 ]; then
      break
  fi
  sleep 2
done
```
### set up env
```
minikube completion bash
```

## start
```
minikube start
```

## uninstall kubectl, uninstall minikube
```
kubectl delete node --all
kubectl delete pods --all
kubectl stop
kubectl delete

launchctl stop '*kubelet*.mount'
launchctl stop localkube.service
launchctl disable localkube.service

sudo kubeadm reset
rm -rf ~/.kube ~/.minikube
sudo rm -rf /usr/local/bin/localkube /usr/local/bin/minikube
sudo rm -rf /etc/kubernetes/

# sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*
sudo apt-get purge kube*
sudo apt-get autoremove

docker system prune -af --volumes
```

## start without VirtualBox/KVM
```
export MINIKUBE_WANTUPDATENOTIFICATION=false
export MINIKUBE_WANTREPORTERRORPROMPT=false
export MINIKUBE_HOME=$HOME
export CHANGE_MINIKUBE_NONE_USER=true

export KUBECONFIG=$HOME/.kube/config
sudo -E minikube start --vm-driver=none
```

## kubectl using minikube context
permanently
```
kubectl config use-context minikube
```

temporary
```
kubectl get pods --context=minikube
```

## check namespaces
```
kubectl get namespaces
```
at least three namespaces will be provided
```
default       Active    15m
kube-public   Active    15m
kube-system   Active    15m
```

## create namespace
```
kubectl create namespace my-own-namespace
```
or via yaml file 
```
kubectl apply -f {filename}
```
```
kind: Namespace
apiVersion: v1
metadata:
  name: test
```

## delete namespace
```
kubectl delete namespace {name of namespace}
```

## get configurations, read configuration in specific format
```
kubectl get configmap 
kubectl get configmap --namespace kube-system 
kubectl get configmap --namespace kube-system kube-proxy --output json
```

## start readiness, check cluster
```
kubectl cluster-info dump
kubectl get node
minikube dashboard
```

## addons
```
minikube addons list
minikube addons enable ingress
```

## deployment
![main schema](https://i.postimg.cc/6pfGpWvN/deployment-high-level.png)

## start dummy container
```
kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.4 --port=8080
```

## create deployment ( with replica set )
```
kubectl run http --image=katacoda/docker-http-server:latest --replicas=1
```

## scale deployment 
```
kubectl scale --replicas=3 deployment {name of the deployment}
```

## create from yaml file, update yaml file
```
kubectl apply -f /path/to/controller.yml
kubectl create -f /path/to/controller.yml
```

## create service fastly
```
kubectl expose deployment helloworld-deployment --type=NodePort --name=helloworld-service
kubectl expose deployment helloworld-deployment --external-ip="172.17.0.13" --port=8000 --target-port=80
```

## reach out service
```
minikube service helloworld-service
minikube service helloworld-service --url
```

## service port range
```
kube-apiserver --service-node-port-range=30000-40000
```

## describe resources
```
kubectl describe deployment {name of deployment}
kubectl describe service {name of service}
```

## describe users, user token
```
kubectl --namespace kube-system describe secret admin-user
```

## get resources
```
kubectl get all --all-namespaces
kubectl get pods
kubectl get pods --namespace kube-system
kubectl get pods --show-labels
kubectl get pods --output=wide --selector="run=load-balancer-example" 
kubectl get service --output=wide
kubectl get service --output=wide --selector="app=helloworld"
kubectl get deployments
kubectl get replicasets
kubectl get nodes
kubectl get cronjobs
kubectl get daemonsets
kubectl get pods,deployments,services,rs,cm,pv,pvc -n demo
```

## determinate cluster 'hostIP' to reach out application(s)
```
minikube ip
```
open 'kube-dns-....'/hostIP
open 'kube-proxy-....'/hostIP

## edit configuration of controller
```
kubectl edit pod hello-minikube-{some random hash}
kubectl edit deploy hello-minikube
kubectl edit ReplicationControllers helloworld-controller
kubectl set image deployment/helloworld-deployment {name of image}
```

## rollout status
```
kubectl rollout status  deployment/helloworld-deployment
```

## rollout history
```
kubectl rollout history  deployment/helloworld-deployment
kubectl rollout undo deployment/helloworld-deployment
kubectl rollout undo deployment/helloworld-deployment --to-revision={number of revision from 'history'}
```

## delete running container
```
kubectl delete pod hello-minikube-6c47c66d8-td9p2
```

## delete deployment
```
kubectl delete deploy hello-minikube
```
## delete ReplicationController
```
kubectl delete rc helloworld-controller
```

## port forwarding from local to pod/deployment/service
next receipts allow to redirect 127.0.0.1:8080 to pod:6379
```
kubectl port-forward redis-master-765d459796-258hz      8080:6379 
kubectl port-forward pods/redis-master-765d459796-258hz 8080:6379
kubectl port-forward deployment/redis-master            8080:6379 
kubectl port-forward rs/redis-master                    8080:6379 
kubectl port-forward svc/redis-master                   8080:6379
```

## persistent volume
```
kind: PersistentVolume
apiVersion: v1
metadata:
  name: pv-volume3
  labels:
    type: local
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data3"
```

to access created volume
```
ls /mnt/data3
```

list of existing volumes
```
kubectl get pv 
kubectl get pvc
```

## Serverless
* OpenFaas
* Kubeless
* Fission
* OpenWhisk


## labels
### show labels for each node
```
kubectl get nodes --show-labels
```

### add label to Node
```
kubectl label nodes {node name} my_label=my_value
```

### remove label from Node
```
kubectl label nodes {node name} my_label-
```

### deploy Pod on Node with label
```
apiVersion: v1
kind: Pod
metadata:
...
spec:
...
  nodeSelector:
    my_label=my_value
```

### create Deployment for specific node
```
apiVersion: some-version
kind: Deployment
metadata:
...
spec:
...
  nodeSelector:
    my_label=my_value
```

### resolving destination node
![when label was not found](https://i.postimg.cc/mDjTpWw3/type-affinity-anti-affinity.png)

* nodeAffinity
* * preferred - deploy in any case, with preferrence my_label=my_value
```
spec:
  affinity:
    nodeAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: my_label
            operator: In
            values:
            - my_value
```
* * required - deploy only when label matched my_label=my_value
```
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: my_label
            operator: In
            values:
            - my_value
```
* nodeAntiAffinity
```
spec:
  affinity:
    nodeAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
```
* podAffinity
* * preferred
spec.affinity.podAffinity.preferredDuringSchedulingIgnoredDuringExecution
* * required
spec.affinity.podAffinity.requiredDuringSchedulingIgnoredDuringExecution
* podAntiAffinity
* * preferred
spec.affinity.podAntiAffinity.preferredDuringSchedulingIgnoredDuringExecution
* * required
spec.affinity.podAntiAffinity.requiredDuringSchedulingIgnoredDuringExecution

## logs
```
kubectl logs <name of pod>
```

## create dashboard 
```
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
```

## access dashboard
```
kubectl -n kube-system describe secret admin-user
http://127.0.0.1:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/overview?namespace=default
kubectl proxy
```

## common
### execute command on specific pod
```
kubectl exec -it {name of a pod}  -- bash -c "echo hi > /path/to/output/test.txt" 
```


# Helm
[documentation](https://docs.helm.sh/)
## Architecture
![main components](https://i.postimg.cc/gkBhFQHG/helm-architecture.png)


## installation
```
sudo snap install helm --classic
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | bash
```

## de-installation
```
helm reset
```

## initialization
```
helm init
# sync latest available packages
helm repo update
```

## useful variables
```
* $HELM_HOME: the location of Helm's configuration
* $TILLER_HOST: the host and port that Tiller is listening on
* $HELM_BIN: the path to the helm command on your system
* $HELM_PLUGIN_DIR: the full path to this plugin (not shown above, but we'll see it in a moment).
```

## analyze local package
```
helm inspect { folder }
helm lint { folder }
```

## search remote package
```
helm search 
helm describe {full name of the package}
```

## information about remote package
```
helm info {name of resource}
helm status {name of resource}
```

## create package locally
```
helm create 
```
![folder structure](https://i.postimg.cc/d1kXZrL7/helm-sceleton.png)

### create package with local templates
```
ls -la ~/.helm/starters/
```

## install package
```
helm install { full name of the package }
helm install --name {my name for new package} { full name of the package }
helm install --name {my name for new package} --namespace {namespace} -f values.yml --debug --dry-run { full name of the package }
```

## install aws plugin
```
helm plugin install https://github.com/hypnoglow/helm-s3.git
```

## list of installed packages
```
helm list
helm list --all
helm ls
```

## package upgrade
local package
```
helm upgrade  {deployment/svc/rs/rc name} . --set replicas=2,maria.db.password="new password"
```
package by name
```
helm upgrade {name of package} {folder with helm scripts} --set replicas=2
```

check upgrade
```
helm history
helm rollback {name of package} {revision of history}
```

## remove packageHelm
```
helm delete --purge {name of package}
```


## trouble shooting
### issue with 'helm list'
```
E1209 22:25:57.285192    5149 portforward.go:331] an error occurred forwarding 40679 -> 44134: error forwarding port 44134 to pod de4963c7380948763c96bdda35e44ad8299477b41b5c4958f0902eb821565b19, uid : unable to do port forwarding: socat not found.
Error: transport is closing
```
solution
```
sudo apt install socat
```

### incompatible version of client and server
```
Error: incompatible versions client[v2.12.3] server[v2.11.0]
```
solution
```
helm init --upgrade
kubectl get pods --namespace kube-system # waiting for start Tiller
helm version
```


## template frameworks
* [go template](https://godoc.org/text/template)
* [sprig template](https://godoc.org/github.com/Masterminds/sprig)
