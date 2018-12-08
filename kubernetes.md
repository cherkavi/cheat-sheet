---
useful links
[task by section](https://kubernetes.io/docs/tasks/)
[k8s examples](https://github.com/wardviaene/kubernetes-course)

---
# Architecture
![nodes with software](https://i.postimg.cc/QCHz6vqH/k8s-architecture.png)
---
# minikube
## minikube installation
```
sudo snap install minikube
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

## check cluster 
```
kubectl cluster-info dump
```

## start dummy container
```
kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.4 --port=8080
```
