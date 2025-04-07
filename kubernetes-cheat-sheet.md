# Kubernetes cheat sheet
## useful links
* [cheat sheet OpenShift](https://github.com/cherkavi/cheat-sheet/tree/master/openshift.md)
* [cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
* [links collections](https://github.com/ramitsurana/awesome-kubernetes)
* [git source](https://github.com/kubernetes/dashboard)
* [architecture](https://kubernetes.io/docs/reference/glossary/?architecture=true)
* [docs task by section](https://kubernetes.io/docs/tasks/)
* [troubleshooting](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-cluster/)
* [java application template generator](http://dekorate.io/)
* [kubernetes patterns](https://k8spatterns.io/)
* [kubernetes dashboard cli dashboard](https://github.com/kdash-rs/kdash/)

## tutorials
* [main tutorial](https://kubernetes.io/docs/tutorials/)
* [tutorial](https://kubernetes.io/docs/tutorials/)
* [tutorial external](https://academy.level-up.one/p/learn-kubernetes-from-a-devops-guru-kubernetes-docker)
* [interactive course](https://www.katacoda.com/courses/kubernetes)
* [interactive course](https://www.katacoda.com/contino/courses/kubernetes)

## local playgrounds
* [kind](#kind)
* [k3s](#k3s)
* [microk8s](#microk8s)
* [minikube](#minikube)

## remote playground and examples
* [playground](https://labs.play-with-k8s.com/)
* [kubeadm cluster](https://killercoda.com/playgrounds/scenario/kubernetes)
* [k8s examples](https://github.com/wardviaene/kubernetes-course)
* [playground](https://www.katacoda.com/courses/kubernetes/playground)
* [ibmcloud](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started)

## tools
* [deployment simplification](https://github.com/cyclops-ui/cyclops)
* [k9s cli manager](https://github.com/derailed/k9s)
   * https://k9scli.io/
* [smart shell for kubectl](https://github.com/cloudnativelabs/kube-shell)
* [log collector](https://github.com/wercker/stern)
* [prompt for K8S](https://github.com/jonmosco/kube-ps1)
* [realtime changes in cluster](https://github.com/pulumi/kubespy)
* [edge router, load balancer](https://doc.traefik.io/traefik/)
* [kubernetes dashboard](https://kubeapps.dev/)
* [k8s manager](https://www.civo.com/kubernetes)

## kubectl installation
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.17.4/bin/linux/amd64/kubectl
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.18.0/bin/linux/amd64/kubectl
```
* kubectl autocompletion
```bash
source <(kubectl completion bash)
# source <(kubectl completion zsh)
```
or
```bash
# source /usr/share/bash-completion/bash_completion
kubectl completion bash >/etc/bash_completion.d/kubectl
```
* trace logging
```bash
rm -rf ~/.kube/cache
kubectl get pods -v=6
kubectl get pods -v=7
kubectl get pods -v=8
# with specific context file from ~/.kube, specific config
kubectl --kubeconfig=config-rancher  get pods -v=8
```
* explain yaml schema
```bash
kubectl explain pods
kubectl explain pods --recursive
kubectl explain pods --recursive --api-version=autoscaling/v2beta1
```
* python client
```bash
pip install kubernetes
```

---
## [Architecture](https://kubernetes.io/docs/concepts/overview/components/)
![architecture](https://i.postimg.cc/RFpnbwgc/k8s-architecture.png)
![architecture](https://i.postimg.cc/ZnbpGZzJ/k8s-architecture-overview.png)
![architecture](https://i.postimg.cc/6pKPX9KY/k8s-architecture-overview.png)
![nodes with software](https://i.postimg.cc/QCHz6vqH/k8s-architecture.png)
![kubernetes](https://i.postimg.cc/CL1Z9Lnv/kubernetes.png)

---
## workflow
![deployment workflow](https://i.postimg.cc/bvJn06Tz/update-workflow.png)

1. The user deploys a new app by using the kubectl CLI. Kubectl sends the request to the API server.
2. The API server receives the request and stores it in the data store (etcd). After the request is written to the data store, the API server is done with the request.
3. Watchers detect the resource changes and send notifications to the Controller to act on those changes.
4. The Controller detects the new app and creates new pods to match the desired number of instances. Any changes to the stored model will be used to create or delete pods.
5. The Scheduler assigns new pods to a node based on specific criteria. The Scheduler decides on whether to run pods on specific nodes in the cluster. The Scheduler modifies the model with the node information.
6. A Kubelet on a node detects a pod with an assignment to itself and deploys the requested containers through the container runtime, for example, Docker. Each node watches the storage to see what pods it is assigned to run. The node takes necessary actions on the resources assigned to it such as to create or delete pods.
7. Kubeproxy manages network traffic for the pods, including service discovery and load balancing. Kubeproxy is responsible for communication between pods that want to interact.

---
## [kind](https://kind.sigs.k8s.io/)
---
## [k3s](https://k3s.io/)
> Lightweight Kubernetes distribution
> For resource-constrained environments ( IoT, Edge, Local-sandbox)
Installation helpers:
* K3D
* k3sup
* kubevip

---
## microk8s
### installation
* https://github.com/ubuntu/microk8s
* https://microk8s.io/

```sh
sudo snap install microk8s --classic
sudo snap install microk8s --classic --edge 
```
### enable addons
```sh
microk8s.start
microk8s.enable dns dashboard
```
### check installation
```sh
microk8s.inspect
```
### check journals for services
```sh
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
## minikube
### installation from snap
```sh
sudo snap install minikube
```
### installation from release
```sh
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && chmod +x kubectl

export MINIKUBE_WANTUPDATENOTIFICATION=false
export MINIKUBE_WANTREPORTERRORPROMPT=false
export MINIKUBE_HOME=$HOME
export CHANGE_MINIKUBE_NONE_USER=true
mkdir $HOME/.kube || true
touch $HOME/.kube/config

# defalue kubernetes config file location
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
#### set up env
```sh
minikube completion bash
```

### start
```sh
minikube start
```

### uninstall kube, uninstall kubectl, uninstall minikube
```sh
kubectl delete node --all
kubectl delete pods --all
kubectl stop
kubectl delete

launchctl stop '*kubelet*.mount'
launchctl stop localkube.service
launchctl disable localkube.service

sudo kubeadm reset
## network cleaining up 
# sudo ip link del cni0
# sudo ip link del flannel.1
# sudo systemctl restart network

rm -rf ~/.kube ~/.minikube
sudo rm -rf /usr/local/bin/localkube /usr/local/bin/minikube
sudo rm -rf /etc/kubernetes/

# sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*
sudo apt-get purge kube*
sudo apt-get autoremove

docker system prune -af --volumes
```


### start without VirtualBox/KVM
```sh
export MINIKUBE_WANTUPDATENOTIFICATION=false
export MINIKUBE_WANTREPORTERRORPROMPT=false
export MINIKUBE_HOME=$HOME
export CHANGE_MINIKUBE_NONE_USER=true

export KUBECONFIG=$HOME/.kube/config
sudo -E minikube start --vm-driver=none
```

### kubectl using minikube context
permanently
```sh
kubectl config use-context minikube
```

temporary
```sh
kubectl get pods --context=minikube
```

### example of started kube processes
```sh
/usr/bin/kubelet 
    --bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf 
    --kubeconfig=/etc/kubernetes/kubelet.conf 
    --config=/var/lib/kubelet/config.yaml 
    --cgroup-driver=cgroupfs 
    --cni-bin-dir=/opt/cni/bin 
    --cni-conf-dir=/etc/cni/net.d 
    --network-plugin=cni 
    --resolv-conf=/run/systemd/resolve/resolv.conf 
    --feature-gates=DevicePlugins=true

kube-apiserver 
    --authorization-mode=Node,RBAC 
    --advertise-address=10.143.226.20 
    --allow-privileged=true 
    --client-ca-file=/etc/kubernetes/pki/ca.crt 
    --disable-admission-plugins=PersistentVolumeLabel 
    --enable-admission-plugins=NodeRestriction 
    --enable-bootstrap-token-auth=true 
    --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt 
    --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key --etcd-servers=https://127.0.0.1:2379 --insecure-port=0 --kubelet-client-certificate=/etc/kubernetes/pki/apiserver-kubelet-client.crt --kubelet-client-key=/etc/kubernetes/pki/apiserver-kubelet-client.key --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname --proxy-client-cert-file=/etc/kubernetes/pki/front-proxy-client.crt --proxy-client-key-file=/etc/kubernetes/pki/front-proxy-client.key --requestheader-allowed-names=front-proxy-client --requestheader-client-ca-file=/etc/kubernetes/pki/front-proxy-ca.crt --requestheader-extra-headers-prefix=X-Remote-Extra- --requestheader-group-headers=X-Remote-Group --requestheader-username-headers=X-Remote-User --secure-port=6443 --service-account-key-file=/etc/kubernetes/pki/sa.pub --service-cluster-ip-range=10.96.0.0/12 --tls-cert-file=/etc/kubernetes/pki/apiserver.crt --tls-private-key-file=/etc/kubernetes/pki/apiserver.key

kube-controller-manager
    --address=127.0.0.1
    --cluster-signing-cert-file=/etc/kubernetes/pki/ca.crt
    --cluster-signing-key-file=/etc/kubernetes/pki/ca.key
    --controllers=*,bootstrapsigner,tokencleaner
    --kubeconfig=/etc/kubernetes/controller-manager.conf
    --leader-elect=true
    --root-ca-file=/etc/kubernetes/pki/ca.crt
    --service-account-private-key-file=/etc/kubernetes/pki/sa.key
    --use-service-account-credentials=true

etcd
    --advertise-client-urls=https://127.0.0.1:2379
    --cert-file=/etc/kubernetes/pki/etcd/server.crt
    --client-cert-auth=true
    --data-dir=/var/lib/etcd
    --initial-advertise-peer-urls=https://127.0.0.1:2380
    --initial-cluster=gtxmachine0=https://127.0.0.1:2380
    --key-file=/etc/kubernetes/pki/etcd/server.key
    --listen-client-urls=https://127.0.0.1:2379
    --listen-peer-urls=https://127.0.0.1:2380
    --name=gtxmachine0
    --peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt
    --peer-client-cert-auth=true
    --peer-key-file=/etc/kubernetes/pki/etcd/peer.key
    --peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
    --snapshot-count=10000
    --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt

kube-scheduler 
    --address=127.0.0.1 
    --kubeconfig=/etc/kubernetes/scheduler.conf 
    --leader-elect=true

/usr/local/bin/kube-proxy 
    --config=/var/lib/kube-proxy/config.conf

/opt/bin/flanneld 
    --ip-masq 
    --kube-subnet-mgr
```

### kubectl using different config file, kubectl config, different config kubectl
```bash
kubectl --kubeconfig=/home/user/.kube/config-student1 get pods
```

### kubectl config with rancher, rancher with kubectl, rancher kubectl config
* certificate-authority-data - from admin account
* token - Bearer Token
```yaml
apiVersion: v1
clusters:
- cluster:
    server: "https://10.14.22.20:9443/k8s/clusters/c-7w47z"
    certificate-authority-data: "....tLUVORCBDRVJUSUZJQ0FURS0tLS0t"
  name: "ev-cluster"

contexts:
- context:
    user: "ev-user"
    cluster: "ev-cluster"
  name: "ev-context"

current-context: "ev-context"

kind: Config
preferences: {}
users:
- name: "ev-user"
  user:  
    token: "token-6g4gv:lq4wbw4lmwtxkblmbbsbd7hc5j56v2ssjvfkxd"
```

## [install on ubuntu, install ubuntu, installation ubuntu, ubuntu installation](https://vitux.com/install-and-deploy-kubernetes-on-ubuntu/)

## update software
```bash
# check accessible list
sudo apt list | grep kube
# update system info
sudo apt-get update
# install one package
sudo apt-get install -y kubeadm=1.18.2-00
```

```sh
# FROM ubuntu:18
# environment
sudo apt install docker.io
sudo systemctl enable docker
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
sudo apt install curl
# kube
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo apt install kubeadm
sudo swapoff -a
# init for using flannel ( check inside kube-flannel.yaml section net-conf.json/Network )
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
rm -rf $HOME/.kube
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
# !!! install flannel ( or weave.... )
# kubectl get nodes
```

## install via rancher
```bash
docker stop rancher
docker rm rancher

# -it --entrypoint="/bin/bash" \
docker run -d --restart=unless-stopped \
  --name rancher \
  -v /var/lib/rancher:/var/lib/rancher \
  -v /var/lib/rancher-log:/var/log \
  -p 9080:80 -p 9443:443 \
  -e HTTP_PROXY="http://qqml:mlfu\$1@proxy.muc:8080" \
  -e HTTPS_PROXY="http://qqml:mlfu\$1@proxy.muc:8080" \
  -e NO_PROXY="localhost,127.0.0.1,127.0.0.0/8,10.0.0.0/8,192.168.0.0/16" \
  rancher/rancher:latest
```


## uninstall
### cleanup node
```bash
# clean up for worker
## !!! most important !!!
sudo rm -rf /etc/cni/net.d

sudo rm -rf /opt/cni/bin
sudo rm -rf /var/lib/kubelet 
sudo rm -rf /var/lib/cni 
sudo rm -rf /etc/kubernetes
sudo rm -rf /run/calico 
sudo rm -rf /run/flannel 

sudo rm -rf /etc/ceph 
sudo rm -rf /opt/rke
sudo rm -rf /var/lib/calico 
sudo rm -rf /var/lib/etcd

sudo rm -rf /var/log/containers 
sudo rm -rf /var/log/pods 

# rancher full reset !!!
sudo rm -rf /var/lib/rancher/*
sudo rm -rf /var/lib/rancher-log/*
```


## [upgrade k8s](https://platform9.com/blog/kubernetes-upgrade-the-definitive-guide-to-do-it-yourself/)

### kube logs 
```sh
### Master
## API Server, responsible for serving the API
/var/log/kube-apiserver.log 
## Scheduler, responsible for making scheduling decisions
/var/log/kube-scheduler.log
## Controller that manages replication controllers
/var/log/kube-controller-manager.log
### Worker Nodes
## Kubelet, responsible for running containers on the node
/var/log/kubelet.log
## Kube Proxy, responsible for service load balancing
/var/log/kube-proxy.log
```

## kubernetes CLI
### kubernetes version, k8s version
```sh
kubeadm version
```
one of the field will be like:
GitVersion:"v1.11.1"

### [kubectl customization, parameters in separate files, kubectl templates ](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/)

### [customization, templating, kustomize, parametrization ](https://kustomize.io/)
```sh
kustomize build config/default | kubectl apply -f-
```
### [customization, templating, config language](https://www.kcl-lang.io/docs/user_docs/getting-started/intro#vs-kustomize)

### kubectl template, inline code
```sh
sed "s|<NODE_INSTANCE_IP>|$NODE_1_IP|" eks-localstorage.yaml-template >  | kubectl apply -f -
```

### access cluster
* reverse proxy 
  activate proxy from current node
  ```sh
  kubectl proxy --port 9090
  # execute request against kubectl via reverse-proxy
  curl {current node ip}:9090/api
  ```
* token access
   ```sh
  $TOKEN=$(kubectl describe secret $(kubectl get secrets | grep ^default | cut -f1 -d ' ') | grep -E '^token' | cut -f2 -d':' | tr -d " ")
  echo $TOKEN | tee token.crt
  echo "Authorization: Bearer "$TOKEN | tee token.header
  # execute from remote node against ( cat ~/.kube/config | grep server )
  curl https://{ip:port}/api --header @token.crt --insecure
  ```
  
### connect to remote machine, rsh
```sh
# connect to remote machine
kubectl --namespace namespace-metrics --kubeconfig=config-rancher exec -ti sm-grafana-deployment-5bdb64-6dnb8 -- /bin/sh
```

### copy from remote machine
```sh
# will fail if no `tar` in container !!!
kubectl --namespace "$KUBECONFIG_ENV" cp "$KUBECONFIG_ENV/$STREAM_POD_NAME:/deployment/app/cloud-application-kafka-streams*.jar" cloud-application-kafka-streams.jar
```

### check namespaces
```sh
kubectl get namespaces
```
at least three namespaces will be provided
```
default       Active    15m
kube-public   Active    15m
kube-system   Active    15m
```

### create namespace
```sh
kubectl create namespace my-own-namespace
```
or via yaml file 
```sh
kubectl apply -f {filename}
```
```yaml
kind: Namespace
apiVersion: v1
metadata:
  name: test
```

### create limits for namespace
example for previous namespace declaration 
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: my-own-namespace
spec:
  limits:
  - default:
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container
```
### limits for certain container
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: frontend
spec:
  containers:
  - name: db
    image: mysql
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: "password"
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```
> also can be limited: pods, pv/pvc, services, configmaps...

### print limits
```sh
kubectl get quota --namespace my-own-namespace
kubectl describe quota/compute-quota --namespace my-own-namespace
kubectl describe quota/object-quota --namespace my-own-namespace

kubectl describe {pod-name} limits 
kubectl describe {pod-name} limits --namespace my-own-namespace
```

## delete namespace
```sh
kubectl delete namespace {name of namespace}
```

## users
* normal user
  * client certificates
  * bearer tokens
  * authentication proxy
  * http basic authentication
  * OpenId
* service user
  * service account tokens
  * credentials using secrets
  * specific to namespace
  * created by objects
  * anonymous user ( not authenticated )

### exeternal applications, user management, managing users
![rbac](https://i.postimg.cc/K84dywbN/k8s-rbac.png)
- https://github.com/sighupio/permission-manager
- https://blog.kubernauts.io/permission-manager-rbac-management-for-kubernetes-ed46c2f38cfb
  be sure that your kube-apiserver is using RBAC autorization
```sh
ps aux | grep kube-apiserver
# expected output
# --authorization-mode=Node,RBAC
```

```bash
# read existing roles 
kubectl get clusterRoles
# describe roles created by permission-management
kubectl describe clusterRoles/template-namespaced-resources___developer
kubectl describe clusterRoles/template-namespaced-resources___operation

# get all rolebindings
kubectl get RoleBinding --all-namespaces
kubectl get ClusterRoleBinding  --all-namespaces
kubectl get rolebindings.rbac.authorization.k8s.io --all-namespaces


# describe one of bindings
kubectl describe ClusterRoleBinding/student1___template-cluster-resources___read-only
kubectl describe rolebindings.rbac.authorization.k8s.io/student1___template-namespaced-resources___developer___students --namespace students 
```

Direct request to api, user management curl
```bash
TOKEN="Authorization: Basic YWRtaW46b2xnYSZ2aXRhbGlp"
curl -X GET -H "$TOKEN" http://localhost:4000/api/list-users
```

## etcd
### etcdctl installation 
untar etcdctl from https://github.com/etcd-io/etcd/releases  
### etcd querying, etcd request key-values
```sh
docker exec -ti `docker ps | grep etcd | awk '{print $1}'` /bin/sh
etcdctl get / --prefix --keys-only
# etcdctl --endpoints=http://localhost:2379 get / --prefix --keys-only
etcdctl  get / --prefix --keys-only | grep permis
etcdctl  get /registry/namespaces/permission-manager -w=json
```


## configuration, configmap
### create configmap
#### example of configuration
```properties
color.ok=green
color.error=red
textmode=true
security.user.external.login_attempts=5
```
#### create configuration on cluster
```sh
kubectl create configmap my-config-file --from-env-file=/local/path/to/config.properties
```
#### will be created next configuration
```yaml
...
data:
color.ok=green
color.error=red
textmode=true
security.user.external.login_attempts=5
```
#### or configuration with additional key, additional abstraction over the properties ( like Map of properties )
```sh
kubectl create configmap my-config-file --from-file=name-or-key-of-config=/local/path/to/config.properties
```
created file is:
```yaml
data:
name-or-key-of-config:
    color.ok=green
    color.error=red
    textmode=true
    security.user.external.login_attempts=5
```
#### or configuration with additional key based on filename ( key will be a name of file )
```sh
kubectl create configmap my-config-file --from-file=/local/path/to/
```
created file is:
```yaml
data:
config.properties:
    color.ok=green
    color.error=red
    textmode=true
    security.user.external.login_attempts=5
```
#### or inline creation
```sh
kubectl create configmap special-config --from-literal=color.ok=green --from-literal=color.error=red
```

### get configurations, read configuration in specific format
```sh
kubectl get configmap 
kubectl get configmap --namespace kube-system 
kubectl get configmap --namespace kube-system kube-proxy --output json
```

### using configuration, using of configmap
* one variable from configmap
  ```yaml
  spec:
    containers:
      - name: test-container
        image: k8s.gcr.io/busybox
        command: [ "/bin/sh", "echo $(MY_ENVIRONMENT_VARIABLE)" ]
        env:
          - name: MY_ENVIRONMENT_VARIABLE
            valueFrom:
              configMapKeyRef:
                name: my-config-file
                key: security.user.external.login_attempts
  ```
* all variables from configmap
   ```yaml
   ...
        envFrom:
        - configMapRef:
            name: my-config-file
   ```

## cluster information
```sh
kubectl cluster-info
kubectl cluster-info dump
```

## start readiness, check cluster
```sh
kubectl get node
kubectl get pods
minikube dashboard
```

## addons
```sh
minikube addons list
minikube addons enable ingress
```

## labels
### show labels for each node
```sh
kubectl get nodes --show-labels
```

### add label to Node
```sh
kubectl label nodes {node name} my_label=my_value
```

### remove label from Node
```sh
kubectl label nodes {node name} my_label-
```


## deployment
to see deployment from external world, remote access to pod, deployment access:  
user ----> Route -----> Service ----> Deployment
![main schema](https://i.postimg.cc/6pfGpWvN/deployment-high-level.png)

### start dummy container
```sh
kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.4 --port=8080
```
### start ubuntu and open shell
```sh
kubectl run --restart=Never --rm -it --image=ubuntu --limits='memory=123Mi' -- sh
```

### create deployment ( with replica set )
```sh
kubectl run http --image=katacoda/docker-http-server:latest --replicas=1
```

### scale deployment 

#### scaling types
* horizontal
* vertical ( scaling up )
#### scaling example
```sh
deployment_name=my_deployment_name
## scale pod to amount of replicas
kubectl scale --replicas=3 deployment $deployment_name

## conditional scaling 
kubectl autoscale deployment $deployment_name --cpu-percent=50 --min=1 --max=3
# check Horizonal Pod Autoscaling
kubectl get hpa 
```

### create from yaml file
```sh
kubectl create -f /path/to/controller.yml
```
### create/update yaml file
```sh
kubectl apply -f /path/to/controller.yml
```

## create service, expose service, inline service fastly
```sh
kubectl expose deployment helloworld-deployment --type=NodePort --name=helloworld-service
kubectl expose deployment helloworld-deployment --external-ip="172.17.0.13" --port=8000 --target-port=80
```
### port forwarding, expose service
```bash
kubectl port-forward svc/my_service 8080 --namespace my_namespace
```

### reach out service
```sh
minikube service helloworld-service
minikube service helloworld-service --url
```

### service port range
```sh
kube-apiserver --service-node-port-range=30000-40000
```

## describe resources, information about resources, inspect resources, inspect pod
```sh
kubectl describe deployment {name of deployment}
kubectl describe service {name of service}
kubectl describe pod {name of the pod}
```

## describe secret, user token
```sh
kubectl --namespace kube-system describe secret admin-user
```
### [External Secret Operator](https://external-secrets.io/latest/)
The operator reads information from external APIs (AWS Secrets Manager, HashiCorp Vault, Google Secrets Manager...)   
and automatically injects the values into a Kubernetes Secret.

## get resources
```sh
kubectl get all --all-namespaces
# check pod statuses 
kubectl get pods
kubectl get pods --namespace kube-system
kubectl get pods --show-labels
kubectl get pods --output=wide --selector="run=load-balancer-example" 
kubectl get pods --namespace training --field-selector="status.phase==Running,status.phase!=Unknown"
kubectl get service --output=wide
kubectl get service --output=wide --selector="app=helloworld"
kubectl get deployments
kubectl get replicasets
kubectl get nodes
kubectl get cronjobs
kubectl get daemonsets
kubectl get pods,deployments,services,rs,cm,pv,pvc -n demo

kubectl list services
kubectl describe service my_service_name
```

## determinate cluster 'hostIP' to reach out application(s)
```sh
minikube ip
```
open 'kube-dns-....'/hostIP
open 'kube-proxy-....'/hostIP

### edit configuration of controller
```sh
kubectl edit pod hello-minikube-{some random hash}
kubectl edit deploy hello-minikube
kubectl edit ReplicationControllers helloworld-controller
kubectl set image deployment/helloworld-deployment {name of image}
```

### rollout status
```sh
kubectl rollout status  deployment/helloworld-deployment
```

### rollout history
```sh
kubectl rollout history  deployment/helloworld-deployment
kubectl rollout undo deployment/helloworld-deployment
kubectl rollout undo deployment/helloworld-deployment --to-revision={number of revision from 'history'}
```

### delete running container
```sh
kubectl delete pod hello-minikube-6c47c66d8-td9p2
```

### delete deployment
```sh
kubectl delete deploy hello-minikube
```
### delete ReplicationController
```sh
kubectl delete rc helloworld-controller
```
### delete PV/PVC
```sh
oc delete pvc/pvc-scenario-output-prod
```

### port forwarding from local to pod/deployment/service
next receipts allow to redirect 127.0.0.1:8080 to pod:6379
```
kubectl port-forward redis-master-765d459796-258hz      8080:6379 
kubectl port-forward pods/redis-master-765d459796-258hz 8080:6379
kubectl port-forward deployment/redis-master            8080:6379 
kubectl port-forward rs/redis-master                    8080:6379 
kubectl port-forward svc/redis-master                   8080:6379
```

### NodeSelector for certain host
```yaml
spec:
   template:
      spec:
         nodeSelector: 
            kubernetes.io/hostname: gtxmachine1-ev
```

### persistent volume
```yaml
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
```sh
ls /mnt/data3
```

list of existing volumes
```sh
kubectl get pv 
kubectl get pvc
```

### [container lifecycle](https://v1-18.docs.kubernetes.io/docs/concepts/containers/container-lifecycle-hooks/)
be aware about conception [Init Container](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) - any pod will be started only when all "init container(s)" will be finished properly
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: lifecycle-demo
spec:
  containers:
  - name: lifecycle-demo-container
    image: nginx
    lifecycle:
      postStart:
        exec:
          command: ["/bin/sh", "-c", "echo Hello from the postStart handler > /usr/share/message"]
      preStop:
        exec:
          command: ["/bin/sh","-c","nginx -s quit; while killall -0 nginx; do sleep 1; done"]
```

```yaml
containers:
  - name: lifecycle
    image: busybox
    lifecycle:
      postStart:
        exec:
          command:
            - "touch"
            - "/var/log/lifecycle/post-start"
      preStop:
        httpGet:
          path: "/abort"
          port: 8080
```

### Serverless
* OpenFaas
* Kubeless
* Fission
* OpenWhisk



### deploy Pod on Node with label
```yaml
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
```yaml
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
  * preferred - deploy in any case, with preferrence my_label=my_value
```yaml
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
  * required - deploy only when label matched my_label=my_value
```yaml
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
```yaml
spec:
  affinity:
    nodeAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
```
* podAffinity
  * preferred
  `spec.affinity.podAffinity.preferredDuringSchedulingIgnoredDuringExecution`
  * required
  `spec.affinity.podAffinity.requiredDuringSchedulingIgnoredDuringExecution`
* podAntiAffinity
  * preferred
  `spec.affinity.podAntiAffinity.preferredDuringSchedulingIgnoredDuringExecution`
  * required
  `spec.affinity.podAntiAffinity.requiredDuringSchedulingIgnoredDuringExecution`

## delete node from cluster
```sh
kubectl get nodes
kubectl delete {node name}
```

## add node to cluster
```sh
ssh {master node}
kubeadm token create --print-join-command  --ttl 0
```
expected result from previous command
```sh
kubeadm join 10.14.26.210:6443 --token 7h0dmx.2v5oe1jwed --discovery-token-ca-cert-hash sha256:1d28ebf950316b8f3fdf680af5619ea2682707f2e966fc0
```
go to node, clean up and apply token
```sh
ssh {node address}
# hard way: rm -rf /etc/kubernetes
kubeadm reset
# apply token from previous step with additional flag: --ignore-preflight-errors=all
kubeadm join 10.14.26.210:6443 --token 7h0dmx.2v5oe1jwed --discovery-token-ca-cert-hash sha256:1d28ebf950316b8f3fdf680af5619ea2682707f2e966fc0 --ignore-preflight-errors=all
```
expected result from previous command
```
...
This node has joined the cluster:
* Certificate signing request was sent to master and a response
  was received.
* The Kubelet was informed of the new secure connection details.
 
Run 'kubectl get nodes' on the master to see this node join the cluster. 
```
next block is not mandatory in most cases
```sh
systemctl restart kubelet
```

## logs
```sh
kubectl logs <name of pod>
```

## create dashboard 
```sh
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
```

## access dashboard
```sh
kubectl -n kube-system describe secret admin-user
http://127.0.0.1:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/overview?namespace=default
kubectl proxy
```

## common
### execute command on specific pod
```sh
kubectl exec -it {name of a pod}  -- bash -c "echo hi > /path/to/output/test.txt" 
```

# Extending 
## [custom controller](https://github.com/kubernetes/sample-controller)

## Weave
```bash
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```

## Flannel
![deployment diagram](https://i.postimg.cc/d3ZwjhXd/flannel.png)
restart nodes
```sh
# remove died pods
kubectl delete pods kube-flannel-ds-amd64-zsfz  --grace-period=0 --force
# delete all resources from file and ignore not found
kubectl delete -f --ignore-not-found https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
kubectl create  -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
install flannel 
```sh
### apply this with possible issue with installation: 
## kube-flannel.yml": daemonsets.apps "kube-flannel-ds-s390x" is forbidden: User "system:node:name-of-my-server" cannot get daemonsets.apps in the namespace "kube-system"
# sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/a70459be0084506e4ec919aa1c114638878db11b/Documentation/kube-flannel.yml

## Container runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized
# sudo kubectl -n kube-system apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml

## print all logs
journalctl -f -u kubelet.service
# $KUBELET_NETWORK_ARGS in 
# /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

## ideal way, not working properly in most cases
sudo kubectl -n kube-system apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

## check installation 
ps aux | grep flannel
# root     13046  0.4  0.0 645968 24748 ?        Ssl  10:49   0:00 /opt/bin/flanneld --ip-masq --kube-subnet-mgr

ifconfig
cni0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 10.244.0.1  netmask 255.255.255.0  broadcast 0.0.0.0
flannel.1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1450
        inet 10.244.0.0  netmask 255.255.255.255  broadcast 0.0.0.0

```

change settings and restart
```sh
kubectl edit cm kube-flannel-cfg -n kube-system
# net-conf.json: | { "Network": "10.244.0.0/16", "Backend": { "Type": "vxlan" } }

# Wipe current CNI network interfaces remaining the old network pool:
sudo ip link del cni0; sudo ip link del flannel.1

# Re-spawn Flannel and CoreDNS pods respectively:
kubectl delete pod --selector=app=flannel -n kube-system
kubectl delete pod --selector=k8s-app=kube-dns -n kube-system

# waiting for restart of all services
```

read logs
```sh
kubectl logs --namespace kube-system kube-flannel-ds-amd64-j4frw -c kube-flannel 
```
read logs from all pods
```sh
for each_node in $(kubectl get pods --namespace kube-system | grep flannel | awk '{print $1}');do echo $each_node;kubectl logs --namespace kube-system $each_node -c kube-flannel;done
```

read settings
```sh
kubectl --namespace kube-system exec kube-flannel-ds-amd64-wc4zp ls /etc/kube-flannel/
kubectl --namespace kube-system exec kube-flannel-ds-amd64-wc4zp cat /etc/kube-flannel/cni-conf.json
kubectl --namespace kube-system exec kube-flannel-ds-amd64-wc4zp cat /etc/kube-flannel/net-conf.json

kubectl --namespace kube-system exec kube-flannel-ds-amd64-wc4zp ls /run/flannel/
kubectl --namespace kube-system exec kube-flannel-ds-amd64-wc4zp cat /run/flannel/subnet.env

kubectl --namespace kube-system exec kube-flannel-ds-amd64-wc4zp ls /etc/cni/net.d
kubectl --namespace kube-system exec kube-flannel-ds-amd64-wc4zp cat /etc/cni/net.d/10-flannel.conflist
```
read DNS logs
```sh
kubectl get svc --namespace=kube-system | grep kube-dns
kubectl logs --namespace=kube-system coredns-78fcd94-7tlpw | tail
```

simple POD, dummy pod, waiting pod
```yaml
kind: Pod
apiVersion: v1
metadata:
  name: sleep-dummy-pod
  namespace: students
spec:
  containers:
    - name: sleep-dummy-pod
      image: ubuntu
      command: ["/bin/bash", "-ec", "while :; do echo '.'; sleep 3600 ; done"]
  restartPolicy: Never
```

## NFS ( Network File System )
### nfs server
```sh
# nfs server 
vim /etc/exports
# /mnt/disks/k8s-local-storage/nfs        10.55.0.0/16(rw,sync,no_subtree_check)
# /mnt/disks/k8s-local-storage1/nfs       10.55.0.0/16(rw,sync,no_subtree_check)

sudo exportfs -a
sudo exportfs -v

systemctl status nfs-server
ll /sys/module/nfs/parameters/
ll /sys/module/nfsd/parameters/
sudo blkid
sudo vim /etc/fstab
# UUID=35c71cfa-6ee2-414a-5555-effc30555555 /mnt/disks/k8s-local-storage ext4 defaults 0 0
# UUID=42665716-1f89-44d4-5555-37b207555555 /mnt/disks/k8s-local-storage1 ext4 defaults 0 0
nfsstat
```
master. mount volume ( nfs server )
```sh
# create point 
sudo mkdir /mnt/disks/k8s-local-storage1
# mount 
sudo mount /dev/sdc /mnt/disks/k8s-local-storage1
sudo chmod 755 /mnt/disks/k8s-local-storage1
# createlink 
sudo ln -s /mnt/disks/k8s-local-storage1/nfs /mnt/nfs1
ls -la /mnt/disks
ls -la /mnt

# update storage
sudo cat /etc/exports
# /mnt/disks/k8s-local-storage1/nfs       10.55.0.0/16(rw,sync,no_subtree_check)

# restart 
sudo exportfs -a
sudo exportfs -v
```

### nfs client
```sh
sudo blkid

sudo mkdir /mnt/nfs1
sudo chmod 777 /mnt/nfs1

sudo vim /etc/fstab
# add record
# 10.55.0.3:/mnt/disks/k8s-local-storage1/nfs /mnt/nfs1 nfs rw,noauto,x-systemd.automount,x-systemd.device-timeout=10,timeo=14 0 0
10.55.0.3:/mnt/disks/k8s-local-storage1/nfs /mnt/nfs1 nfs defaults 0 0

# refresh fstab
sudo mount -av

# for server 
ls /mnt/disks/k8s-local-storage
ls /mnt/disks/k8s-local-storage1

# for clients
ls /mnt/disks/k8s-local-storage1
```

### trouble shooting, problem resolving
```sh
POD_NAME=service-coworking-postgresql-0

kubectl get pod $POD_NAME -o json
kubectl describe pod $POD_NAME

kubectl get --watch events
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl get events --field-selector type=Warning
kubectl get events --field-selector type=Error
kubectl get events --field-selector type=Critical

kubectl get pvc data-service-coworking-postgresql-0 -o json
```

### postgresql   waiting for a volume to be created
[create provider](https://github.com/parjun8840/ekscsidriver/blob/main/README.md#2-get-oidc-provider-url-and-create-an-identity-provider)
```sh
kubectl get all -l app.kubernetes.io/name=aws-ebs-csi-driver -n kube-system
```
### certificate is expired
```sh
bootstrap.go:195] Part of the existing bootstrap client certificate is expired: 2019-08-22 11:29:48 +0000 UTC
```
solution
```bash
sudo cat /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

# archive configuration
sudo cp /etc/kubernetes/pki /etc/kubernetes/pki_backup
sudo mkdir /etc/kubernetes/conf_backup
sudo cp /etc/kubernetes/*.conf /etc/kubernetes/conf_backup

# remove certificates
sudo rm /etc/kubernetes/pki/./apiserver-kubelet-client.crt 
sudo rm /etc/kubernetes/pki/./etcd/healthcheck-client.crt 
sudo rm /etc/kubernetes/pki/./etcd/server.crt 
sudo rm /etc/kubernetes/pki/./etcd/peer.crt 
sudo rm /etc/kubernetes/pki/./etcd/ca.crt 
sudo rm /etc/kubernetes/pki/./front-proxy-client.crt 
sudo rm /etc/kubernetes/pki/./apiserver-etcd-client.crt 
sudo rm /etc/kubernetes/pki/./front-proxy-ca.crt 
sudo rm /etc/kubernetes/pki/./apiserver.crt 
sudo rm /etc/kubernetes/pki/./ca.crt 
sudo rm /etc/kubernetes/pki/apiserver.crt 
sudo rm /etc/kubernetes/pki/apiserver-etcd-client.crt 
sudo rm /etc/kubernetes/pki/apiserver-kubelet-client.crt 
sudo rm /etc/kubernetes/pki/ca.crt 
sudo rm /etc/kubernetes/pki/front-proxy-ca.crt 
sudo rm /etc/kubernetes/pki/front-proxy-client.crt 

# remove configurations
sudo rm /etc/kubernetes/apiserver-kubelet-client.*
sudo rm /etc/kubernetes/front-proxy-client.*
sudo rm /etc/kubernetes/etcd/*
sudo rm /etc/kubernetes/apiserver-etcd-client.*
sudo rm /etc/kubernetes/admin.conf
sudo rm /etc/kubernetes/controller-manager.conf
sudo rm /etc/kubernetes/kubelet.conf
sudo rm /etc/kubernetes/scheduler.conf

# re-init certificates
sudo kubeadm init phase certs all --apiserver-advertise-address {master ip address} --ignore-preflight-errors=all

# re-init configurations
sudo kubeadm init phase kubeconfig all --ignore-preflight-errors=all

# re-start
sudo systemctl stop kubectl.service
sudo systemctl restart docker.service
docker system prune -af --volumes
reboot

# /usr/bin/kubelet
sudo systemctl start kubectl.service

# init locate kubectl
sudo cp /etc/kubernetes/admin.conf ~/.kube/config

# check certificate
openssl x509 -in /etc/kubernetes/pki/apiserver.crt  -noout -text  | grep "Not After"
```

## template frameworks
* [go template](https://godoc.org/text/template)
* [sprig template](https://godoc.org/github.com/Masterminds/sprig)


## Troubleshooting
### issue with PV / PVC
```
pod has unbound immediate PersistentVolumeClaims. preemption: 0/2 nodes are available: 2 No preemption victims found for incoming pod..
```
create - check PV & PVC capacities (requested space) - PVC should have not more than PV
