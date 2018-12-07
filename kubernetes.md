---
useful links
[task by section](https://kubernetes.io/docs/tasks/)

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
minikube start --vm-driver=none
```
## kubectl using minikube context
```
kubectl config use-context minikube
kubectl get pods --context=minikube
```
