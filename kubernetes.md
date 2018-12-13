---
useful links
[task by section](https://kubernetes.io/docs/tasks/)
[k8s examples](https://github.com/wardviaene/kubernetes-course)

---
# Architecture
![architecture](https://i.postimg.cc/TwZs4CN0/k8s-architecture-overview.png)
![nodes with software](https://i.postimg.cc/QCHz6vqH/k8s-architecture.png)
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
```
kubectl config use-context minikube
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

## start dummy container
```
kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.4 --port=8080
```

## create from yaml file, update yaml file
```
kubectl apply -f /path/to/controller.yml
kubectl create -f /path/to/controller.yml
```

## create service fastly
```
kubectl expose deployment helloworld-deployment --type=NodePort --name=helloworld-service
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

## labels
### add labels to Node
```
kubectl label nodes {node ip/name} my_label=my_value
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


# Helm
## Architecture
![main components](https://i.postimg.cc/gkBhFQHG/helm-architecture.png)

## installation
```
sudo snap install helm --classic
helm init
```
## de-installation
```
helm reset
```

## commands
* helm install
* helm search 
* helm list
* helm upgrade
* helm rollback 

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

## template frameworks
* [go template](https://godoc.org/text/template)
* [sprig template](https://godoc.org/github.com/Masterminds/sprig)
