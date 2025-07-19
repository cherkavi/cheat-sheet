# Docker cheat sheet
## links
* [online playground](https://labs.play-with-docker.com)
* [commands description](https://tutorialshub.org/docker-commands/)
* [Docker Tutorial](https://dev.to/aurelievache/understanding-docker-part-1-retrieve-pull-images-3ccn)
* [docker debug](https://github.com/ktock/buildg)
* [docker dashboard](https://github.com/benphelps/homepage/pkgs/container/homepage)
* [container optimized OS](https://fedoraproject.org/coreos/)
* [server management platform VM's and Containers with Web Interface](https://www.proxmox.com/en/proxmox-virtual-environment/overview)

### Ecosystem
* Docker Daemon
* Docker Volume - store persist data
* Docker Client - CLI, gui analogue ( Kitematic )
* Docker Compose - Python app over Docker Daemon
* Docker Swarm
* Docker HUB
* Docker Cloud
### [docker code checker](https://www.checkov.io/2.Basics/Installing%20Checkov.html)

### installation ( Debian )
[docker desktop](https://docs.docker.com/desktop/install/ubuntu/)
```sh
sudo apt install docker.io
# start daemon on Debian
sudo systemctl start docker
sudo systemctl enable docker
```
```sh
#!/bin/bash
apt-get update
apt-get install -y apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
apt-get update
apt-get install -y docker-ce

usermod -aG docker ubuntu
docker run -p 8080:8080 tomcat:8.0
```

#### docker manager
* [docker awesome tools](https://github.com/veggiemonk/awesome-docker)
* [docker ui tool ui manager](https://github.com/jesseduffield/lazydocker)
  ```sh
  go install github.com/jesseduffield/lazydocker@latest
  ```
* https://github.com/portainer/portainer  
### bash completion
```sh
curl -o ~/.docker-machine.bash  https://raw.githubusercontent.com/docker/machine/master/contrib/completion/bash/docker-machine.bash 
```
update your bashrc
```sh
. ~/.docker-machine.bash
```

### Architecture
![architecture](https://i.postimg.cc/jqXQSC12/docker-architecture.png)

### Tools
* [layers explorer](https://github.com/wagoodman/dive)
* [layers, config, copy image between diff registries ... ](https://github.com/containers/skopeo/blob/main/install.md)
  possible issue on ubuntu: https://github.com/cherkavi/cheat-sheet/blob/master/linux.md#issue-with-go-package-installation
* [copy image between registries](oras.land)


### information about docker itself
```sh
docker info
docker system info
```

### settings files
```
/etc/docker/daemon.json
/etc/default/docker 
~/.docker/config.json
/etc/systemd/system/docker.service.d/http-proxy.conf
```

### how to skip typing "sudo" each time, without sudo
```sh
# new group in sudo for docker
sudo groupadd docker
# add current user into docker group
sudo usermod -aG docker $USER 

# restart service
sudo service docker restart
# restart daemon
systemctl daemon-reload
# refresh sudo 
sudo reboot
```

Docker Issue:
---
```
Couldn't connect to Docker daemon at http+docker://localhost - is it running?
```
```sh
sudo usermod -a -G docker $USER
sudo systemctl enable docker # Auto-start on boot
sudo systemctl start docker # Start right now
# reboot
```

---
```
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock
```
logout and login again

---
```
standard_init_linux.go:228: exec user process caused: exec format error
```
```dockerfile
WORKDIR /opt/airflow
ENTRYPOINT ["./entrypoint.sh"]
```
> also can be helpful to check entrypoint for UNIX (LF) as new line ( instead of windows CR LF )

## proxy set up:
### proxy for daemon
* /etc/systemd/system/docker.service.d/10_docker_proxy.conf
```
[Service]
Environment=HTTP_PROXY=http://1.1.1.1:111
Environment=HTTPS_PROXY=http://1.1.1.1:111
```
or
```
[Service]    
Environment="HTTP_PROXY=http://webproxy.host.de:3128/" "NO_PROXY=localhost,127.0.0.1,.host.de,.viola.local,.local"
```

```bash
sudo shutdown -r now
```
* /etc/sysconfig/docker
```
HTTP_PROXY="http://user01:password@10.10.10.10:8080"
HTTPS_PROXY="https://user01:password@10.10.10.10:8080"
```

### proxy for docker client to pass proxy to containers
* ~/.docker/config.json
```
{
 "proxies":
 {
   "default":
   {
     "httpProxy": "http://127.0.0.1:3001",
     "noProxy": "*.test.example.com,.example2.com"
   }
 }
}
```
```bash
# restart docker with daemon
sudo systemctl daemon-reload && sudo systemctl restart docker
# or
sudo systemctl restart docker.service
# or 
sudo service docker restart
```



* docker run --env-file environment.file {image name}
> ( or via -e variables )

```
HTTP_PROXY=http://webproxy.host:3128
http_proxy=http://webproxy.host:3128
HTTPS_PROXY=http://webproxy.host:3128
https_proxy=http://webproxy.host:3128
NO_PROXY="localhost,127.0.0.1,.host.de,.viola.local"
no_proxy="localhost,127.0.0.1,.host.de,.viola.local"
```
* /etc/default/docker
```
export http_proxy="http://host:3128/"
```

* if all previous options not working ( due permission ) or you need to execute (apt install, wget, curl, ... ):
  * build arguments
  ```bash
  sudo docker build \
  --build-arg rsync_proxy=http://$TSS_USER:$TSS_PASSWORD@proxy.muc:8080 \
  --build-arg https_proxy=https://$TSS_USER:$TSS_PASSWORD@proxy.muc:8080 \
  --build-arg http_proxy=http://$TSS_USER:$TSS_PASSWORD@proxy.muc:8080 \
  --build-arg no_proxy=localhost,127.0.0.1,.localdomain,.ubsroup.net,.ubs.corp,.cn.sub,.muc,.vantage.org \
  --build-arg ftp_proxy=http://$TSS_USER:$TSS_PASSWORD@proxy.muc:8080 \
  --file Dockerfile-firefox .
  ```
  * change Docker file with additional lines ( not necessary, only for earlier docker version )
  ```Dockerfile
  ARG rsync_proxy
  ENV rsync_proxy $rsync_proxy
  ARG http_proxy
  ENV http_proxy $http_proxy
  ARG no_proxy
  ENV no_proxy $no_proxy
  ARG ftp_proxy
  ENV ftp_proxy $ftp_proxy
  ...
  # at the end of file
  unset http_proxy
  unset ftp_proxy
  unset rsync_proxy
  unset no_proxy
  ```

## login, logout
```
docker login -u cherkavi -p `oc whoami -t` docker-registry.local.org
docker logout docker-registry.local.org
# for artifactory you can use token as password
```
check login
```sh
cat ~/.docker/config.json | grep "auth\":" | awk -F '"' '{print $4}' | base64 -d -
```
check login without access to config
```sh
echo "" | docker login docker-registry.local.org
echo $?
```

issue
```
...
6c01b5a53aac: Waiting 
2c6ac8e5063e: Waiting 
cc967c529ced: Waiting 
unauthorized: authentication required
```
solution
```
rm -rf ~/.docker/config.json
# or just remove one record inside "auths" block with your url-repo
docker logout url-repo
```

## restart Docker service
```
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo systemctl show docker
# restart docker
sudo systemctl restart docker.service
```


Images
------
### search image into registry, find image, catalog search
```sh
docker search <text of search>
```

### inspect image in repository inspect layers analyse image show layers image xray
```sh
## skopeo
# sudo apt-get -y install skopeo
# or: https://ftp.de.debian.org/debian/pool/main/s/skopeo/
skopeo inspect docker://registry.fedoraproject.org/fedora:latest
# show all executed command lines list of commands in docker container 
skopeo inspect --config docker://registry.fedoraproject.org/fedora:latest | grep -e "CMD" -e "ENTRYPOINT"

## dive
# https://github.com/wagoodman/dive
dive ${DOCKER_REGISTRY}/portal-production/jenkins-builder

## [docker scout](https://docs.docker.com/scout/)
export PATH=$HOME/.docker/cli-plugins:$PATH
source <(docker-scout completion bash)
```

### export layers tag layers
```sh
docker save imagename build-layer1 build-layer2 build-layer3 > image-caching.tar
docker load -i image-caching.tar
```

### export layers to filesystem
```sh
# https://github.com/larsks/undocker/
docker save busybox | undocker -vi -o busybox -l 
```

### pull image from repository
```sh
docker pull <image name>
docker pull cc-artifactory.myserver.net/some-path/<image name>:<image version>
```
> image can be found: https://hub.docker.com/
> example of command: docker pull mysql

### push image to repository
```sh
docker push cc-artifactory.myserver.net/some-path/<image name>:<image version>
```

### copy images between registries
```sh
skopeo copy docker://quay.io/buildah/stable docker://registry.internal.company.com/buildah
```

### show all local images
```sh
docker images --all
docker image list --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}\t{{.Size}}"
docker image list --format "{{.ID}}\t{{.Repository}}"
```


## Run and Start
---
### [run container automatically, run on start, autorun, autostart ](https://docs.docker.com/config/containers/start-containers-automatically/)
```
docker run -d --restart=unless-stopped {CONTAINER ID}
# docker update --restart=no {CONTAINER ID}
```
|value|description|
|:----:|------------|
| no |	Do not automatically restart the container. (the default) |
| on-failure |	Restart the container if it exits due to an error, which manifests as a non-zero exit code. |
| always |	Always restart the container if it stops. If it is manually stopped, it is restarted only when Docker daemon restarts or the container itself is manually restarted. (See the second bullet listed in restart policy details) |
| unless-stopped |	Similar to always, except that when the container is stopped (manually or otherwise), it is not restarted even after Docker daemon restarts. |

------
### map volume ( map folder )
```
-v {host machine folder}:{internal folder into docker container}:{permission}
```
```
-v `pwd`:/home/root/host_folder:rw
-v $PWD:/home/root/host_folder:Z
-v /tmp:/home/root/tmp
```

### check volumes
```
docker volume ls
```

### map multiply ports to current host
```
-p {host machine port}:{internal docker machine port}
```
```sh
docker run -d -p 8030-8033:8030-8033/tcp  e02d9b40e89d
docker run -d -p 8040:8040/tcp  prom/prometheus 
```

### run container in detached ( background ) mode, without console attachment to running process
* --detach
* -d=true
* -d

### run image with specific name
```sh
docker run --name my_specific_name {name of image}
```

### run image with specific user ( when you have issue with rights for mounting external folders )
```sh
docker run --user root {name of image}
```
### run image with current user ( when you use docker image as pre-installed software )
```sh
docker run --user $(id -u):$(id -g) {name of the image}
```

### run container with empty entrypoint, without entrypoint
```dockerfile
ENTRYPOINT []
```
```sh
docker run --entrypoint="" --interactive --tty image-name /bin/sh 
```


### start stopped previously container
```
docker start {CONTAINER ID}
```

## Connect containers
### connecting containers via host port, host connection
```sh
# external data storage for Redis: --volume /docker/host/dir:/data
sudo docker run --publish 7001:6379 --detach redis
# ip a | grep docker -B 2 | grep inet | grep global
sudo docker run --interactive --tty redis redis-cli -h 172.17.0.1 -p 7001
```

### connecting containers directly via link
```sh
sudo docker run --name my-redis-container --detach redis
sudo docker run --interactive --tty --name my-redis-cli --link my-redis-container:redis redis redis-cli -h redis -p 6379
```

### connecting containers via network
```sh
docker network create some-network

docker run --network some-network --name my-redis -d redis
docker run --network some-network --interactive --tty redis redis-cli -h my-redis
```

### share network for docker-compose
```sh
docker network create --driver bridge my-local
docker network inspect my-local
docker network rm my-local
```

```yaml
version: '3'
services:
    ...

networks:
    default:
        external:
            name: heritage-local
```

### host address
```
172.17.0.1
```
or
with .bashrc: ``` export DOCKER_GATEWAY_HOST=172.17.0.1 ```
```yaml
# docker-compose.yml
version: '3.7'

services:
  app:
    image: your-app:latest
    ports:
      - "8080:8080"
    environment:
      DB_UPSTREAM: http://${DOCKER_GATEWAY_HOST:-host.docker.internal}:3000
```

### connecting containers via host, localhost connection, shares the host network stack and has access to the /etc/hosts for network communication, host as network share host network share localhost network
```sh
docker run --rm   --name postgres-docker -e POSTGRES_PASSWORD=docker -d -p 5432:5432 postgres
# --net=host 
# --network host 
docker run -it --rm --network="host" postgres psql -h 0.0.0.0 -U postgres
```

### assign static hostname to container (map hostname)
* create network
```
docker network create --subnet=172.18.0.0/16 docker.local.network
```
* assign address with network
```
docker run --net docker.local.network --ip 172.18.0.100 --hostname hadoop-local --network-alias hadoop-docker -it {CONTAINER ID} /bin/bash
```
* check network
```
docker inspect {CONTAINER ID} | grep -i NETWORK
```

### network types
```
--network="bridge" : 
  'host': use the Docker host network stack
  'bridge': create a network stack on the default Docker bridge
  'none': no networking
  'container:<name|id>': reuse another container's network stack
  '<network-name>|<network-id>': connect to a user-defined network
```
## mount folder, map folder, mount directory, map directory multiple directories
```sh
working_dir="/path/to/working/folder"
docker run --volume $working_dir:/work -p 6900-6910:5900-5910 --name my_own_container -it ubuntu:18.04 /bin/sh
# !!! path to the host folder should be absolute !!! attach current folder 
docker run --entrypoint="" --name airflow_custom_local --interactive --tty --publish 8080:8080 --volume `pwd`/logs:/opt/airflow/logs --volume `pwd`/dags:/opt/airflow/dags airflow_custom /bin/sh 
```

## Volumes
### create volume
```
docker volume create {volume name}
```

### inspect volume, check volume, read data from volume, inspect data locally
```
docker volume inspect {volume name}
```
```json
[
    {
        "CreatedAt": "2020-03-12T22:07:53+01:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/snap/docker/common/var-lib-docker/volumes/cd72b76daf3c66de443c05dfde77090d5e5499e0f2a0024f9ae9246177b1b86e/_data",
        "Name": "cd72b76daf3c66de443c05dfde77090d5e5499e0f2a0024f9ae9246177b1b86e",
        "Options": null,
        "Scope": "local"
    }
]
```
```sh
# inspect Mountpoint
ls -la /var/snap/docker/common/var-lib-docker/volumes/cd72b76daf3c66de443c05dfde77090d5e5499e0f2a0024f9ae9246177b1b86e/_data
```

### list of all volumes
```
docker volume ls
```

### using volume
```
docker run {name of image} -v {volume name}:/folder/inside/container
docker run {name of image} -mount source={volume name},target=/folder/inside/container
```


Inspection
------
### show all containers that are running
```
docker ps
```

### show all containers ( running, stopped, paused )
```
docker ps -a
```

### show container with filter, show container with format
* [output formatting](https://docs.docker.com/config/formatting/)
* [default values for output](https://github.com/BrianBland/docker/blob/master/api/client/formatter/formatter.go#L19)
```
# filter docker images by name 
# output format - names with commands (https://github.com/BrianBland/docker/edit/master/api/client/formatter/formatter.go)
docker ps -a --filter "name=redis-lab" --format "{{.Names}} {{.Command}}"
docker ps -a --filter "name=redis-lab" --format "{{.Names}} {{.Command}}"
docker ps -a --format "  {{.ID}} {{.Names}}"
```

### join to executed container, connect to container, rsh, sh on container
```
docker attach {CONTAINER ID}
# docker exec --interactive --tty {CONTAINER ID} /bin/sh
```
with detached sequence
```
docker attach {CONTAINER ID} --detach-keys="ctrl-Z"
```
with translation of all signals ( detaching: ctrl-p & ctrl-q )
```
docker attach {CONTAINER ID} --sig-proxy=true
```


### docker log of container, console output
```
docker logs --follow --tail all {CONTAINER ID}
docker logs --follow --tail 25 {CONTAINER ID}
docker logs {CONTAINER ID}
docker logs --since 10m {CONTAINER ID}
docker logs --since 2018-01-01T00:00:00 {CONTAINER ID}
```

### show processes from container
```
docker top {CONTAINER ID}

# https://github.com/bcicen/ctop
# sudo apt-get install docker-ctop
ctop
```

### run program inside container and attach to process
```
docker exec -it {CONTAINER ID} /bin/bash
```

### show difference with original image
```
docker diff {CONTAINER ID}
```

### show all layers command+size, reverse engineering of container, print dockerfile
```
docker history --no-trunc {CONTAINER ID}
docker image history --no-trunc {CONTAINER ID}
```

### docker running image information
```
docker inspect
docker image inspect
docker inspect -f '{{.HostConfig.PortBindings}}' {CONTAINER ID}
```

### debug information
```
docker --debug
```
or for file /etc/docker/daemon.json
```
{
  "debug": true
}
```

### [copy files between container or host](https://docs.docker.com/engine/reference/commandline/cp/)
```sh
docker cp <src> <cont:dest>
```

## save
------
### docker save changed container commit changes fix container changes
```sh
docker run --entrypoint="" -it {IMAGE_ID} /bin/sh
# execute some commands like `apt install curl`....
```
> make a changes and keep it running
> select another terminal window
```sh
docker ps
# select proper container_id 
docker commit {CONTAINER_ID} {NEW_IMAGE_NAME}
```
> !!! be aware, in case of skipping entrypoint, in committed image with be no entrypoint too

### container new name, rename container, container new tag
```
# change name of container
docker tag {IMAGE_ID} <TAG_NAME[:TAG VERSION]>
docker tag {TAG_1} {TAG_2}
# untag
docker rmi {TAG_NAME}
```

### docker save - image with layers and history
```
docker save --output <output file name>.tar {CONTAINER ID}
```

### docker export - container WITHOUT history, without layers
```
docker export --output <output file name>.tar {CONTAINER ID}
```

## Load/Import/Read from file to image
------
### load full image into 'images' - with all layers and history
```
docker load -i {filename of archive}
```

### import full image into 'images' - like a basement 
```
docker import {filename of archive}
```

## Stop and Pause
------
### wait until container will be stopped
```
docker wait {CONTAINER ID}
```

### stop executing container
```
docker stop {CONTAINER ID}
```

### stop restarted container, compose stop, stop autostart, stop restarting
```
docker update --restart=no {CONTAINER ID}
# send signal SIGTERM
docker stop {CONTAINER ID}
```

### pause/unpause executing container
```
docker pause {CONTAINER ID}
docker unpause {CONTAINER ID}
```

### kill executing container
```
# send signal SIGKILL
docker kill {CONTAINER ID}
# send signal to container
docker kill --signal=9 {CONTAINER ID}
```

### leave executing container
```
just kill the terminal
```

## Remove and Clean, docker cleanup
------
### remove all containers
```sh
docker rm `docker ps -a | awk -F ' ' '{print $1}'`
```

### remove image
```sh
docker rmi <IMAGE ID>
docker rmi --force <IMAGE ID>
# remove unused images
sudo docker rmi `sudo docker images | grep "<none>" | awk '{print $3}'`
```

### remove volumes ( unused )
```
docker volume ls -qf dangling=true | xargs -r docker volume rm
```

### cleanup docker
```sh
docker system prune -af --volumes
# clean only unused volumes
docker system prune -f --volumes
```
### delete
```
docker network ls  
docker network ls | grep "bridge"   
docker network rm $(docker network ls | grep "bridge" | awk '/ / { print $1 }')
```

### delete docker, remove docker, uninstall docker
```
sudo docker network ls

sudo apt remove docker.io
sudo rm -rf /etc/systemd/system/docker.service
sudo rm -rf /etc/systemd/system/docker.socket
rm /home/$USER/.docker/.buildNodeID
sudo rm -rf /var/lib/docker
```

Additional management
------
### docker events real time
```
docker system events
```

### disk usage infomration
```
docker system df
```

### remove unused data, remove stopped containers
```
docker system prune
```

### [UI manager](https://portainer.readthedocs.io)
```
docker pull portainer/portainer
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer
```
login/pass: admin/12345678

## installation issues
```
The following packages have unmet dependencies:
 docker-ce : Depends: libseccomp2 (>= 2.3.0) but 2.2.3-3ubuntu3 is to be installed
E: Unable to correct problems, you have held broken packages.
```
resolution:
```
sudo apt install docker-ce=17.03.0~ce-0~ubuntu-xenial
```

---
```
Error starting daemon: error initializing graphdriver: /var/lib/docker contains several valid graphdrivers: overlay2, aufs; Please cleanup or explicitly choose storage driver (-s <DRIVER>)
Failed to start Docker Application Container Engine.
```
resolution
```
sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual
sudo modprobe aufs
sudo gedit /lib/systemd/system/docker.service &
ExecStart=/usr/bin/dockerd -H fd:// --storage-driver=aufs
```

---
possible issue with 'pull'
```
Error response from daemon: Get https://registry-1.docker.io/v2/: dial tcp: lookup registry-1.docker.io on 160.55.52.52:8080: no such host
```

---
build error
```
W: Failed to fetch http://archive.ubuntu.com/ubuntu/dists/xenial/InRelease  Could not resolve 'archive.ubuntu.com'
W: Failed to fetch http://archive.ubuntu.com/ubuntu/dists/xenial-updates/InRelease  Could not resolve 'archive.ubuntu.com'
```
need to add proxy into Dockerfile
```
ENV http_proxy http://user:passw@proxy.url:8080
ENV https_proxy http://user:passw@proxy.url:8080

```

## Build
* docker cache: `docker build --no-cache`
* docker base image: `FROM ...`
* docker ignore files - ignore files from build process: `.dockerignore`
* docker build time argument `docker build --build-arg KEY=VALUE`
* docker runtime variables `docker run --env KEY=VALUE` 
* 
### build from file
```
docker build -t {name of my own image}:latest {name of docker file | . } --no-cache
docker build -t solr-4.10.3:latest . // Dockerfile into current folder
docker build --tag java-app-runner:latest --build-arg http_proxy=http://user:passw@proxy.zur:8080  --file /home/projects/current-task/mapr/Dockerfile .
```

### build with parameters, build with proxy settings
```
build --build-arg app_name=k8s-ambassador
docker build --build-arg http_proxy=proxy.muc:8080 --build-arg https_proxy=proxy.muc:8080 .
```

### build with parameters inside dockerfile 
```Dockerfile
ARG app_name
ENV JAR=$app_name.jar
```

```Dockerfile
# bash command
# docker build --tag rviz-image --build-arg ROS_VERSION=latest .
# 
ARG ROS_VERSION
FROM cc-artifactory.ubsgroup.com/docker/ros:${ROS_VERSION}
```

dockerfile with pre-build two FROM section
```Dockerfile
# Build node app
FROM node:16 as build
WORKDIR /src
RUN yarn install --immutable
RUN yarn run web:build:prod

# start file service
FROM caddy:2.5.2-alpine
WORKDIR /src
COPY --from=build /src/web/.webpack ./
EXPOSE 80
CMD ["caddy", "file-server", "--listen", ":80"]
```

### build useful commands
| command |   description |
|---------|---------------|
| FROM | Sets the base image, starting image to build the container, must be first line|
| MAINTAINER  | Sets the author field of the generated images|
| RUN |  Execute commands in a new layer on top of the current image and commit the results|
| CMD |  Allowed only once (if many then last one takes effect)|
| LABEL |  Adds metadata to an image|
| EXPOSE |  Informs container runtime that the container listens on the specified network ports at runtime|
| ENV |  Sets an environment variable|
| ADD |  Copy new files, directories, or remote file URLs from into the filesystem of the container|
| COPY |  Copy new files or directories into the filesystem of the container|
| ENTRYPOINT |  Allows you to configure a container that will run as an executable|
| VOLUME |  Creates a mount point and marks it as holding externally mounted volumes from native host or other containers|
| USER |  Sets the username or UID to use when running the image|
| WORKDIR |  Sets the working directory for any RUN, CMD, ENTRYPOINT, COPY, ADD commands|
| ARG |  Defines a variable that users can pass at build-time to the builder using --build-arg|
| ONBUILD |  Adds an instruction to be executed later, when the image is used as the base for another build|
| STOPSIGNAL |  Sets the system call signal that will be sent to the container to exit|

> Use RUN instructions to build your image by adding layers 
> Use ENTRYPOINT to CMD when building executable Docker image and you need a command always to be executed. 
> ( ENTRYPOINT can be re-writed from command-line: docker run -d  -p 80:80 --entrypoint /bin/sh alipne )
> Use CMD if you need to provide extra default arguments that could be overwritten from command line when docker container runs.
> Use CMD if you need to provide       default arguments that could be overwritten from command line when docker container runs.                         

### push your container
* docker login <registry name>
* docker tag <name of the container> <dockerhub username>/<name of the container>
  also possible notation: <registry name>/<repository name>:<tag>
* docker push <dockerhub username>/<name of the container>
```
DOCKER_REGISTRY="default-image-registry.apps.vantage.org"
IMAGE_LOCAL="ab1023fb0ac8"
OC_PROJECT="stg-1"
DOCKER_IMAGE_LOCAL_REPO=local
DOCKER_IMAGE_LOCAL_TAG=drill_connector

sudo docker tag $IMAGE_LOCAL $DOCKER_REGISTRY/$OC_PROJECT/$DOCKER_LOCAL_REPO:$DOCKER_LOCAL_TAG
sudo docker push $DOCKER_REGISTRY/$OC_PROJECT/$DOCKER_LOCAL_REPO:$DOCKER_LOCAL_TAG

```

### advices
* for a starting points ( FROM ) using -alpine or -scratch images, for example: "FROM python:3.6.1-alpine"
* Each line in a Dockerfile creates a new layer, and because of the layer cache, the lines that change more frequently, for example, adding source code to an image, should be listed near the bottom of the file.
* CMD will be executed after COPY
* microdnf - minimal package manager
```
FROM python:3.6.1-alpine
RUN pip install flask
CMD ["python","app.py"]
COPY app.py /app.py
```
* create user and group, create group
```
RUN groupadd -g 2053 r-d-ubs-technical-user
RUN useradd -ms /bin/bash -m -u 2056 -g 2053 customer2description
# activate user
USER customer2description
```
* for downloading external artifacts need to use ADD command, COPY vs ADD
```
# download file
ADD http://artifactory.com/sourcefile.txt  /destination/path/sourcefile.txt
# download and extract archive
ADD https://artifactory.com/source.file.tar.gz /temp
```

### read labels from container, read container labels, LABEL commands from container
```sh
docker inspect  --format '{{ .Config.Labels }}' cc-artifactory.ubsroup.net/docker/ros-automation:latest
```

## [docker sdk](https://docs.docker.com/engine/api/sdk/) 
communication with dockerd via REST & Python & CLI 
### docker sdk rest api 
```sh
docker_api_version=1.41
# get list of containers
curl --unix-socket /var/run/docker.sock http://localhost/v${docker_api_version}/containers/json
# start container by image id
docker_image_id=050db1833a9c
curl --unix-socket /var/run/docker.sock -X POST http://localhost/v${docker_api_version}/containers/${docker_image_id}/start
```
### [docker sdk python](https://github.com/cherkavi/python-utilities/blob/master/docker/docker_list.py)

## Examples
### simple start
```sh
docker run -it ubuntu /bin/sh
docker exec -it high_mclean /bin/bash
```
### docker with env variables
```sh
docker run --env http_proxy=$http_proxy --env https_proxy=$https_proxy --env no_proxy=$no_proxy -it ubuntu:18.04 /bin/bash
```
### docker with local network
```sh
docker run -v /tmp:/home/root/tmp --net docker.local.network --ip 172.18.0.100 --hostname hadoop-local --network-alias hadoop-docker -t -i  -p  50075:50075/tcp  -p 50090:50090/tcp sequenceiq/hadoop-docker /etc/bootstrap.sh -bash
```
### docker with extension rights
```sh
docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 7180 4239cd2958c6 /usr/bin/docker-quickstart
```

### MariaDB
#### MariaDB start in container
```sh
docker run --detach --env MYSQL_ROOT_PASSWORD=root --env MYSQL_USER=root --env MYSQL_PASSWORD=root --env MYSQL_DATABASE=technik_db --name golang_mysql --publish 3306:3306 mysql;

docker run --name mysql-container --volume /my/local/folder/with/data:/var/lib/mysql --volume /my/local/folder/with/init/scripts:/docker-entrypoint-initdb.d --publish 3306:3306 --env MYSQL_DATABASE=activitidb --env MYSQL_ROOT_PASSWORD=root --detach mariadb --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```
#### MariaDB sql dump creation:
```sh
docker exec mysql-container sh -c 'exec mysqldump --all-databases -uroot -p"$MYSQL_ROOT_PASSWORD"' > /some/path/on/your/host/all-databases.sql
```
#### MariaDB sql dump import
```sh
docker run --net=host -v /some/path/on/your/host:/sql -it arey/mysql-client --host=10.143.242.65 --port=3310 --user=root --password=example --database=files -e "source /sql/all-databases.sql"
```

# docker compose

## [installation](https://github.com/docker/compose/releases)
```
chmod +x docker-compose-Linux-x86_64
sudo mv docker-compose-Linux-x86_64 /usr/local/bin/docker-compose
sudo apt-get install  --only-upgrade docker
```
## check installation from python
```python
import docker
import compose
```

## variables in compose file
```docker
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    # name of container for compose
    container_name: app_admin
    ports:
      - "8081:80"
    environment:
      - PMA_HOST=mysql
      - PMA_PORT=${MYSQL_PORT}
      - PMA_USER=${MYSQL_USER}
      - PMA_PASSWORD=${MYSQL_PASSWORD}
    depends_on: 
      - mariadb
```
and file ```.env``` in the same folder
```properties
MYSQL_USER=joomla
MYSQL_PASSWORD=joomla
MYSQL_PORT=3306
```
```sh
docker-compose config
```
	
## dependecies inheritance
```yaml
version: '3'


base-airflow-common:
  &airflow-common
  build:
    context: .
    dockerfile: .docker/airflow-init.Dockerfile
  env_file:
      - .docker/.env
  volumes:
    - ./airflow-dag/wondersign_airflow_shopify/dags:/opt/airflow/dags
    - ./logs:/opt/airflow/logs
  depends_on:
    redis:
      condition: service_healthy
    db_variant:
      condition: service_healthy


services:

  airflow-webserver:
    <<: *airflow-common
    command: webserver
    ports:
      - 8080:8080
    healthcheck:
      test: ["CMD", "curl", "--fail", "http://localhost:8080/health"]
      interval: 10s
      timeout: 10s
      retries: 5
    restart: always	
```
remove entry point
```
entrypoint:
    - php
    - -d
    - zend_extension=/usr/local/lib/php/xdebug.so
    - -d
    - memory_limit=-1
    - vendor/bin/phpunit
```
check memory limits inside container
```sh
# outside
docker stats <container_name>
# inside
cat /sys/fs/cgroup/memory/memory.limit_in_bytes
cat /sys/fs/cgroup/memory/memory.max_usage_in_bytes
```
	
## start in detached mode, up and detach
```
docker-compose up -d
```

## start with re-creating all containers
```bash
docker-compose up --force-recreate
# start without re-creation
docker-compose up --no-recreate
```

## start compose with additional variables
```
docker-compose -f docker-compose.yaml build --force-rm --build-arg MY_LOCAL_VAR
```

## start compose with multiply files
```
docker-compose -f docker-file1.yaml -f docker-file2.yaml -f docker-file3.yaml up
```

## docker-compose find folder with image
default name of image contains name of the folder like a prefix
( but underscore and minus signs can be removed )

## Issues
```
In file './docker-compose.yml' service 'version' doesn't have any configuration options.
```
solution: 
* check format of the docker-compose file
* install docker-copmose respective your Docker version
---
```
ERROR: error while removing network: dockerairflow_default
```

```
docker ps
# dockerairflow_webserver_1 
# dockerairflow_postgres_1 

docker network disconnect --force dockerairflow_default dockerairflow_webserver_1 
docker network disconnect --force dockerairflow_default dockerairflow_postgres_1 
# docker network rm --force dockerairflow_default

sudo aa-status
sudo systemctl disable apparmor.service --now
sudo aa-status
sudo apt-get purge --auto-remove apparmor
sudo service docker restart
docker system prune --all --volumes
```

## Docker hack docker change files docker volumes location manual volume edit 
```sh
docker volume inspect
# ls /var/lib/docker/volumes/4a6b2fa5a102985d377e545d6cb8648ed4f80da2ae835a1412eb02b9e0c03a52/_data
```
```sh
docker image inspect puckel/docker-airflow:1.10.9
# find "UpperDir", "LowerDir"
# ls /var/lib/docker/overlay2/97d76c31dd4907544b7357c3904853f9ceb3c755a5dedd933fee44491d9ec900/diff
vim /var/lib/docker/overlay2/97d76c31dd4907544b7357c3904853f9ceb3c755a5dedd933fee44491d9ec900/diff/usr/local/airflow/airflow.cfg
```


# Docker swarm
[gossip protocol](https://en.wikipedia.org/wiki/Gossip_protocol)
![swarm overview](https://i.postimg.cc/LsgrnfYJ/swarm-overview.png)
## init 'manager' node
```
docker swarm init --advertise-addr eth0
```
you will see invitation to add 'worker' node like
```
docker swarm join --token SWMTKN-1-3p93jlhx2hx9wif8xphl6e47c5ukwz12a00na81g7h0uopk6he-6xof1chqhjuor7hkn65ggjw1p 192.168.0.18:2377
```

to show token again
```
docker swarm join-token worker
docker swarm join-token manager
```

amount of managers:
* Three manager nodes tolerate one node failure.
* Five manager nodes tolerate two node failures.
* Seven manager nodes tolerate three node failure
amount of worker nodes - hundreds, thousands!!!

to leave docker cluster
```
docker swarm leave
```

to print amount of nodes into cluster
```
docker node ls
docker node inspect {node name}
```
## create service ( for manager only )
```
docker service create --detach=true --name nginx1 --publish 80:80  --mount source=/etc/hostname,target=/usr/share/nginx/html/index.html,type=bind,ro nginx:1.12
```
* request to each node will be routed to routing mesh
* each docker container on each node will have own mount point ( you should see name of different hosts from previous example )


## inspect services
```
docker service ls
```

## inspect service
```
docker service ps {service name}
```

## update service, change service attributes
```
docker service update --replicas=5 --detach=true {service name}
docker service update --image nginx:1.13 --detach=true nginx1
```
* store desire state into internal storage
* swarm recognized diff between desired and current state
* tasks will be executed according diff

## service log
log will be aggregated into one place and can be shown
```
docker service log
```

## docker pause
```
docker pause {container name}
docker unpause {container name}
```


## routing mesh effect
```
The routing mesh built into Docker Swarm means that any port that is published at the service level will be exposed on every node in the swarm. Requests to a published service port will be automatically routed to a container of the service that is running in the swarm.
```

# docker daemon
```sh
## start docker daemon process
sudo dockerd 
# start in debug mode
sudo dockerd -D 
# start in listening mode
sudo dockerd -H 0.0.0.0:5555 

# using client with connection to remove docker daemon
docker -H 127.0.0.1:5555 ps
```


# issues
## docker image contain your local proxy credentials, remove credentials from docker container
container that you built locally contains your proxy credentials
```
# execution inside container
docker exec -it {container name} /bin/sh
env
# showing your credentials
```
*solution*
* remove your credentials from file ~/.docker/config.json
```json
{
 { proxies:{
     default: {httpProxy: "your value"}
   }
  }
}
```
* build container with "--build-arg"
```sh
docker build \
--tag $BUILD_IMAGE_NAME  \
--build-arg http_proxy=$http_proxy \
--build-arg https_proxy=$https_proxy \
--build-arg no_proxy=$no_proxy \
.
```

## docker login
```
Error response from daemon: Get https://docker-registry-default.dplapps.adv.org/v2/: x509: certificate signed by unknown authority
```
* *solution1* - skip authentication 
change file ~/.docker/config.json
```
...
	"auths": {
		"docker-registry-default.dplapps.adv.org": {},
		"https://docker-registry-default.dplapps.adv.org": {}
	},
...
```
* *solution2* - authentication
```bash
url_to_registry="docker-registry-default.simapps.advant.org"
sudo mkdir -p "/etc/docker/certs.d/$url_to_registry"
sudo cp certificate.crt /etc/docker/certs.d/$url_to_registry

docker login -u login_user -p `oc whoami -t` $url_to_registry
```

## docker push, docker pull
```
authentication required
```
*solution*
before 'docker login' need change file ~/.docker/config.json remove next block
```
    "credsStore": "secretservice"
```

## docker instance issue
```bash
#apt install software-properties-common
Reading package lists... Done
Building dependency tree       
Reading state information... Done
E: Unable to locate package software-properties-common
```
*solution*
need to execute 'update' before new package installation
```
apt update
```
and also helpful
```
apt install -y software-properties-common
```

## docker build command issue
**issue**
```
FROM cc.ubsgroup.net/docker/builder
RUN mkdir /workspace
COPY dist/scenario_service.pex /workspace/scenario_service.pex
WORKDIR /workspace
```
```sh
docker build -t local-scenario --file Dockerfile-scenario-file .
# COPY/ADD failed: stat /var/lib/docker/tmp/docker-builder905175157/scenario_service.pex: no such file or directory
```
but file exists and present in proper place

*solution 1*
check your ```.dockerignore ``` file for ignoring your "dist" or even worse "*" files :
```.dockerignore
# ignore all files
*
```

*solution 2*
```
FROM cc.ubsgroup.net/docker/builder
RUN mkdir /workspace
COPY scenario_service.pex /workspace/scenario_service.pex
WORKDIR /workspace
```

```
FULL_PATH="/home/projects/adp"
DOCKER_FILE="Dockerfile-scenario"
BUILD_SUBFOLDER="dist"
IMAGE_NAME="local-scenario"
docker build -t $IMAGE_NAME --file $FULL_PATH/$DOCKER_FILE $FULL_PATH/$BUILD_SUBFOLDER
```
## docker remove volume issue
```
Error response from daemon: remove typo3_db_data: volume is in use 
```
```
docker system prune -af --volumes
docker volume rm typo3_db_data
```


# [RKT](https://coreos.com/rkt/docs/latest/)
* [getting started fastly](https://coreos.com/blog/getting-started-with-rkt-1-0.html)
* [gettings started](https://coreos.com/rkt/docs/latest/getting-started-guide.html)
* [documenation](https://coreos.com/rkt/docs/latest/)


# [Podman](https://podman.io/)
* [podman desktop manager](https://podman-desktop.io/)
* [podman commands](https://docs.podman.io/en/latest/Commands.html)
```sh
sudo apt install podman
```
# [Buildah](https://github.com/containers/buildah)


# Kaniko
> the tool to build container images from a Dockerfile without needing a Docker daemon
* [how to install it in K8S](https://cloud.google.com/blog/products/containers-kubernetes/introducing-kaniko-build-container-images-in-kubernetes-and-google-container-builder-even-without-root-access)
* [github](https://github.com/GoogleContainerTools/kaniko)