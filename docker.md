Docker
======

### information about docker itself
```
docker info
docker system info
```


### how to skip typing "sudo" each time
1. sudo groupadd docker
2. sudo usermod -aG docker $USER
( add current user into docker group )
3. sudo service docker restart



## proxy set up:
* /etc/default/docker
```
export http_proxy="http://host:3128/"
```
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

* /etc/systemd/system/docker.service.d/http-proxy.conf
```
[Service]    
Environment="HTTP_PROXY=http://webproxy.host.de:3128/" "NO_PROXY=localhost,127.0.0.1,.host.de,.viola.local,.local"
```
* /etc/systemd/system/docker.service.d/10_docker_proxy.conf
```
[Service]
Environment=HTTP_PROXY=http://1.1.1.1:111
Environment=HTTPS_PROXY=http://1.1.1.1:111
```

## restart Docker service
```
sudo systemctl daemon-reload
sudo systemctl restart docker
```


Images
------
### search image into registry
```
docker search <text of search>
```

### pull image from repository
```
docker pull <image name>
```
> image can be found: https://hub.docker.com/
> example of command: docker pull mysql

### show all local images
```
docker images --all
```


## Run and Start
------
### map volume ( map folder )
```
-v {host machine folder}:{internal folder into docker container}
```
```
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
```
docker run -d -p 8030-8033:8030-8033/tcp  e02d9b40e89d
docker run -d -p 8040:8040/tcp  prom/prometheus 
```

### run container in detached ( background ) mode, without console attachment to running process
* --detach
* -d=true
* -d

### run image with specific name
```
docker run --name my_specific_name {name of image}
```

### run image with specific user ( when you have issue with rights for mounting external folders )
```
docker run --user root {name of image}
```

### start stopped previously container
```
docker start {CONTAINER ID}
```

## Volumes
### create volume
```
docker volume create {volume name}
```

### inspect volume, check volume
```
docker volume inspect {volume name}
```
( "Mountpoint" will show real position )

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

### join to executed container
```
docker attach {CONTAINER ID}
```

### docker log of container, console output
```
docker logs --follow --tail 25 {CONTAINER ID}
docker logs {CONTAINER ID}
```

### show processes from container
```
docker top {CONTAINER ID}
```

### run program inside container and attach to process
```
docker exec -it {CONTAINER ID} /bin/bash
```

### show difference with original image
```
docker diff {CONTAINER ID}
```

### show all layers command+size
```
docker history --no-trunc {CONTAINER ID}
```

### docker running image information
```
docker inspect
docker inspect -f '{{.HostConfig.PortBindings}}' {CONTAINER ID}
```

## Save
------
### docker save/commit
```
docker commit {CONTAINER ID} <new image name>
```

### docker save/commit
```
docker tag {CONTAINER ID} <TAG NAME[:TAG VERSION]>
```

### docker save - with layers and history
```
docker save --output <output file name>.tar {CONTAINER ID}
```

### docker export - image WITHOUT history, without layers
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

### pause/unpause executing container
```
docker pause {CONTAINER ID}
docker unpause {CONTAINER ID}
```

### kill executing container
```
docker kill {CONTAINER ID}
```

### leave executing container
```
just kill the terminal
```

## Remove and Clean
------
### remove all containers
```
docker rm `docker ps -a | awk -F ' ' '{print $1}'`
```

### remove image
```
docker rmi <IMAGE ID>
docker rmi --force <IMAGE ID>
```

### remove volumes ( unused )
```
docker volume ls -qf dangling=true | xargs -r docker volume rm
```

### delete
```
docker network ls  
docker network ls | grep "bridge"   
docker network rm $(docker network ls | grep "bridge" | awk '/ / { print $1 }')
```

Additional management
------
### disk usage infomration
```
docker system df
```

### remove unused data, remove stopped containers
```
docker system prune
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

### shares the host network stack and has access to the /etc/hosts for network communication
```
--net=host 
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

### build from file
```
docker build -t {name of my own image}:latest {name of docker file | . }
docker build -t solr-4.10.3:latest . // Dockerfile into current folder
```

### build with parameters
```
build --build-arg app_name=k8s-ambassador
```

inside Dockerfile
```
ARG app_name
ENV JAR=$app_name.jar
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

### [online playground](https://labs.play-with-docker.com)

### advices
* for a starting points ( FROM ) using *-alpine images, for example: "FROM python:3.6.1-alpine"

Examples
------
* docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 7180 4239cd2958c6 /usr/bin/docker-quickstart
* docker exec -it high_mclean /bin/bash
* docker run -v /tmp:/home/root/tmp --net docker.local.network --ip 172.18.0.100 --hostname hadoop-local --network-alias hadoop-docker -t -i  -p  50075:50075/tcp  -p 50090:50090/tcp sequenceiq/hadoop-docker /etc/bootstrap.sh -bash
* docker run --detach --env MYSQL_ROOT_PASSWORD=root --env MYSQL_USER=root --env MYSQL_PASSWORD=root --env MYSQL_DATABASE=technik_db --name golang_mysql --publish 3306:3306 mysql;
* MariaDB
```
docker run --name mysql-container --volume /my/local/folder/with/data:/var/lib/mysql --volume /my/local/folder/with/init/scripts:/docker-entrypoint-initdb.d --publish 3306:3306 --env MYSQL_DATABASE=activitidb --env MYSQL_ROOT_PASSWORD=root --detach mariadb --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
```

* MariaDB sql dump creation:
```
docker exec mysql-container sh -c 'exec mysqldump --all-databases -uroot -p"$MYSQL_ROOT_PASSWORD"' > /some/path/on/your/host/all-databases.sql
```
