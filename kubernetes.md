---
useful links
[task by section](https://kubernetes.io/docs/tasks/)
[k8s examples](https://github.com/wardviaene/kubernetes-course)

---
# Architecture
![architecture](https://i.postimg.cc/TwZs4CN0/k8s-architecture-overview.png)
![nodes with software](https://i.postimg.cc/QCHz6vqH/k8s-architecture.png)
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

## get configuration 
```
kubectl get pods
```

## edit deploy
```
kubectl edit pod hello-minikube-{some random hash}
kubectl edit deploy hello-minikube
```
## delete running container
```
kubectl delete pod hello-minikube-6c47c66d8-td9p2
```
# Helm
## Architecture
![main components](https://i.postimg.cc/gkBhFQHG/helm-architecture.png)

## template frameworks
* [go template](https://godoc.org/text/template)
* [sprig template](https://godoc.org/github.com/Masterminds/sprig)
