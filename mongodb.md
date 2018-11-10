### original docker container
```
docker pull mongo
```

### run container from image
``` 
# unexpectedly not working:  -e MONGO_INITDB_ROOT_USERNAME=vitalii -e MONGO_INITDB_ROOT_PASSWORD=vitalii
docker run -d --name mongo -p 27017:27017 -p 28017:28017 -v /tmp/mongo/db:/data/db mongo
```

### connect to existing container and execute 'mongo' tool
* execute via bash connection
```
docker exec -it {containerID} /bin/sh
find / -name 'mongo'
mongo --host 127.0.0.1 --port 27017 -u my_user -p my_password --authenticationDatabase my_db
```
* exec command directly 
```
docker exec -it {containerID} mongo --host 127.0.0.1 -u my_user -p my_password --authenticationDatabase my_db
```

### create user via bash, mongo eval, execute commands from bash 
export MONGO_USER=vitalii
export MONGO_PASS=vitalii
mongo admin --eval "db.createUser({user: '$MONGO_USER', pwd: '$MONGO_PASS', roles:[{role:'root',db:'admin'}]});"

### change password via 'mongo' tool
db.changeUserPassword(username, password)

### start via docker-compose
```
version: '3.1'
services:
  mongo:
    image: mongo
    restart: always
    ports:
      - 27017:27017
      - 28017:28017
    volumes:
      - /home/technik/projects/ista/mongo-docker/container-map-folder:/data/db
    environment:
      MONGO_INITDB_ROOT_USERNAME: vitalii
      MONGO_INITDB_ROOT_PASSWORD: vitalii

  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - 28018:8081
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: vitalii
      ME_CONFIG_MONGODB_ADMINPASSWORD: vitalii
```

### discover environment, meta-information  'mongo' tool
| commands | description |
| -------- | ----------- |
| show dbs | Shows all databases available on this server |
| use acmegrocery |  Switches to a database called acmegrocery. Creates this database if it doesn’t already exist. |
| show collections |  Show all collections in the current db (first `use <someDb>`) |
| show users |  Show all users for the current DB |
| show roles |  Show the roles defined for the current DB |
