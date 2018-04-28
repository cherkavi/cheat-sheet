Docker
======

### information about docker itself
docker info


### how to skip typing "sudo" each time
1. sudo groupadd docker
2. sudo usermod -aG docker $USER
( add current user into docker group )
3. sudo service docker restart



Images
------

### search image into registry
docker search <text of search>

### pull image from repository
docker pull <image name>
> image can be found: https://hub.docker.com/
> example of command: docker pull mysql

### show all local images
docker images --all


## Run and Start
------

### map volume ( map folder )
-v /tmp:/home/root/tmp

### check volumes
docker volume ls

### map multiply ports to current host
-p 8030-8033:8030-8033/tcp  -p 8040:8040/tcp

### run container in detached ( background ) mode, without console attachment to running process
* --detach
* -d=true

### run image with specific name
docker run --name my_specific_name {name of image}

### start stopped previously container
docker start {CONTAINER ID}

## Volumes
### create volume
docker volume create {volume name}

### inspect volume, check volume
docker volume inspect {volume name}
( "Mountpoint" will show real position )

### list of all volumes
docker volume ls

### using volume
docker run {name of image} -v {volume name}:/folder/inside/container
docker run {name of image} -mount source={volume name},target=/folder/inside/container


Inspection
------

### show all containers that are running
docker ps

### show all containers ( running, stopped, paused )
docker ps -a

### join to executed container
docker attach {CONTAINER ID}

### docker log of container
### console output
docker logs --follow --tail 25 {CONTAINER ID}

### show processes from container
docker top {CONTAINER ID}

### run program inside container and attach to process
docker exec -it {CONTAINER ID} /bin/bash

### show difference with original image
docker diff {CONTAINER ID}

### show all layers command+size
docker history --no-trunc {CONTAINER ID}

### docker running image information
docker inspect

docker inspect -f '{{.HostConfig.PortBindings}}' {CONTAINER ID}


Save
------
### docker save/commit
docker commit {CONTAINER ID} <new image name>

### docker save/commit
docker tag {CONTAINER ID} <TAG NAME[:TAG VERSION]>

### docker export
docker save --output <output file name>.tar {CONTAINER ID}


Stop and Pause
------

### wait until container will be stopped
docker wait {CONTAINER ID}

### stop executing container
docker stop {CONTAINER ID}

### pause/unpause executing container
docker pause {CONTAINER ID}

docker unpause {CONTAINER ID}

### kill executing container
docker kill {CONTAINER ID}

### leave executing container
just kill the terminal


Remove and Clean
------
### remove all containers
docker rm `docker ps -a | awk -F ' ' '{print $1}'`

### remove image
docker rmi <IMAGE ID>

docker rmi --force <IMAGE ID>

### remove volumes ( unused )
docker volume ls -qf dangling=true | xargs -r docker volume rm

### delete
$ docker network ls  

$ docker network ls | grep "bridge"   

$ docker network rm $(docker network ls | grep "bridge" | awk '/ / { print $1 }')


Additional management
------

### assign static hostname to container (map hostname)
* create network
docker network create --subnet=172.18.0.0/16 docker.local.network
* assign address with network
docker run --net docker.local.network --ip 172.18.0.100 --hostname hadoop-local --network-alias hadoop-docker -it {CONTAINER ID} /bin/bash
* check network
docker inspect {CONTAINER ID} | grep -i NETWORK

### UI manager
docker pull portainer/portainer
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer
> https://portainer.readthedocs.io
login/pass: admin/12345678

Examples
------
* docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 7180 4239cd2958c6 /usr/bin/docker-quickstart
* docker exec -it high_mclean /bin/bash
* docker run -v /tmp:/home/root/tmp --net docker.local.network --ip 172.18.0.100 --hostname hadoop-local --network-alias hadoop-docker -t -i  -p  50075:50075/tcp  -p 50090:50090/tcp sequenceiq/hadoop-docker /etc/bootstrap.sh -bash
* docker run --detach --env MYSQL_ROOT_PASSWORD=root --env MYSQL_USER=root --env MYSQL_PASSWORD=root --env MYSQL_DATABASE=technik_db --name golang_mysql --publish 3306:3306 mysql;
