# [Foxglove](https://github.com/foxglove/studio)

## clone repo
```sh
git clone --depth 1 https://github.com/foxglove/studio
# Syntax Error: Unexpected identifier
# (function (exports, require, module, __filename, __dirname) { version https://git-lfs.github.com/spec/v1
cd studio
git lfs pull
```

## local run
```sh
NODE_VERSION=16.15.0
docker run --volume $PWD:/app -it node:$NODE_VERSION /bin/bash
cd /app
# node --version
corepack enable
yarn install --immutable
yarn run web:build:prod
```

## docker 
### local build
build container locally
```sh
FOXGLOVE_IMAGE_NAME=foxglove-local
docker build -t $FOXGLOVE_IMAGE_NAME .
docker save --output $FOXGLOVE_IMAGE_NAME.tar $FOXGLOVE_IMAGE_NAME:latest
# docker save --output $FOXGLOVE_IMAGE_NAME-cors.tar $FOXGLOVE_IMAGE_NAME:cors
docker load -i {filename of archive}
```

build container locally and use CORS for the application
```sh
FOXGLOVE_IMAGE_NAME=foxglove-local
FOXGLOVE_CADDY_CONF=caddy-cors.conf
echo ':8080
root * /src
file_server browse

# https://caddyserver.com/docs/caddyfile/directives/header
header Access-Control-Allow-Origin "*"

header { 
  Access-Control-Allow-Origin "*"
  Access-Control-Allow-Methods "GET, POST, PUT"
  Access-Control-Allow-Headers "Content-Type"
}' > $FOXGLOVE_CADDY_CONF

# check execution
# docker run  --entrypoint="" --publish 9090:8080 -v `pwd`:/localdata -v `pwd`/caddy-cors.conf:/etc/caddy/Caddyfile $FOXGLOVE_IMAGE_NAME caddy run --config /etc/caddy/Caddyfile --adapter caddyfile

FOXGLOVE_DOCKER_CORS=Dockerfile-cors
echo "FROM foxglove-local
COPY $FOXGLOVE_CADDY_CONF /etc/caddy/Caddyfile
WORKDIR /src
EXPOSE 8080
CMD [\"caddy\",\"run\",\"--config\",\"/etc/caddy/Caddyfile\",\"--adapter\",\"caddyfile\"]" > $FOXGLOVE_DOCKER_CORS

docker build -t $FOXGLOVE_IMAGE_NAME:cors -f $FOXGLOVE_DOCKER_CORS .

docker run --publish 9090:8080 $FOXGLOVE_IMAGE_NAME:cors
```
