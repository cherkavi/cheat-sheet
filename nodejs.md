# node

## installation
* [install NodeJS](https://github.com/nodejs/help/wiki/Installation)
* [node multiversion manager](https://github.com/nvm-sh/nvm)
* [install Angular](https://cli.angular.io/)

## tools
* [process manager with load balancer](https://www.npmjs.com/package/pm2)
* [execute package manager](https://www.npmjs.com/package/npx)

### run different node version
#### run different node versions locally
```sh
sudo npm install -g n 
n ls
sudo n install 13.8
n exec 13.8 node --version
n exec 20.3 node --version
```

#### run node in docker, docker running, run node with version
[node docker tags](https://hub.docker.com/_/node/tags)  
```sh
# NODE_VERSION=16.15.0
NODE_VERSION=14.21.1-alpine
docker run --volume $PWD:/app -it node:$NODE_VERSION /bin/bash
cd /app
node --version
```

## command line arguments 
### RUNNING YOUR CODE
```sh
# Evaluates the current argument as JavaScript
node --eval
# Checks the syntax of a script without executing it
node --check
# Opens the node.js REPL (Read-Eval-Print-Loop)
node --interactive
# Pre-loads a specic module at start-up
node --require
# Silences the deprecation warnings
node --no-deprecation
# Silences all warnings (including deprecations)
node --no-warnings
# Environment variable that you can use to set command line options
echo $NODE_OPTIONS
```
### CODE HYGIENE
```sh
# Emits pending deprecation warnings
node --pending-deprecation
# Prints the stack trace for deprecations
node --trace-deprecation
Throws error on deprecation
node --throw-deprecation
Prints the stack trace for warnings
node --trace-warnings
```

### INITIAL PROBLEM INVESTIGATION
```sh
# Generates node report on signal
node --report-on-signal
# Generates node report on fatal error
node --report-on-fatalerror
# Generates diagnostic report on uncaught exceptions
node --report-uncaught-exception
```

### CONTROLLING/INVESTIGATING MEMORY USE
```sh
# Sets the size of the heap
--max-old-space-size
# Turns on gc logging
--trace_gc
# Enables heap proling
--heap-prof
# Generates heap snapshot on specied signal
--heapsnapshot-signal=signal
```

### CPU PERFORMANCE INVESTIGATION
```sh
# Generates V8 proler output.
--prof
# Process V8 proler output generated using --prof
--prof-process
# Starts the V8 CPU proler on start up, and write the CPU prole to disk before exit
--cpu-prof
```

### DEBUGGING
```sh
# Activates inspector on host:port and break at start of user script
--inspect-brk[=[host:]port]
# Activates inspector on host:port (default: 127.0.0.1:9229)
--inspect[=[host:]port]
```

# npm
## config
```sh
npm config ls
npm config list
```
## npm registry
```sh
# how to set registry
npm config set registry https://registry.npmjs.org/
npm config delete registry
```
or adjust environment
```sh
NPM_CONFIG_REGISTRY=https://registry.npmjs.org/
```
## proxy
```sh
npm config get https-proxy
npm config set https-proxy [url:port]
```
## proxy in .npmrc
```sh
vim ~/.npmrc
```
```properties
proxy=http://user:passw$@proxy.muc:8080/
https-proxy=http://user:passw$@proxy.muc:8080/
;prefix=~/.npm-global
```
## npm version
```sh
npm info coa
npm info coa versions
```

## check installation
```sh
npm bin -g
```

## package control
```sh
# best practice
# package-lock.json must have be present in root ( under git control )
npm ci
```

## permission denied for folder /usr/lib
```sh
# create new folder where node will place all packages
mkdir ~/.npm-global

# Configure npm to use new folder
npm config set prefix '~/.npm-global'

# update your settings in ```vim ~/.profile```
export PATH=~/.npm-global/bin:$PATH
```

## [npm commands](https://docs.npmjs.com/cli-documentation/cli)

## print high level packages
```sh
npm list -g --depth=0
```

## reinstall package globally
```sh
npm search @angular

# full package name
npm uninstall -g @angular/cli
# uninstall by name 
# npm uninstall -g fx

npm cache clear --force
npm install -g @angular/cli 
```

## show package version
```
npm show styled-components@* version
```

## install package version
```sh
npm install styled-components@5.2.1
# if you don't know certain version
npm install styled-components@^3.0.0
```
## install with package registry
```sh
npm install needle@2.9.1 --registry=https://artifactory.ubs.net/artifactory/api/npm/external-npmjs-org/ --force
```

## build project ( install dependencies )
```sh
npm install
npm start
```

## start from different folder, start with special marker
```sh
npm start --prefix /path/to/api "special_app_marker_for_ps_aux"
```

## start with different port
* package.json solution
```json
 "scripts": {"start": "PORT=3310 node ./bin/www"},
```
* npm solution
```
PORT=$PORT npm --prefix $PROJECT_HOME/api start 
PORT=$PORT npm --prefix $PROJECT_HOME/api run start 
```

## eject configuration to static files
```sh
npm run eject
# config/webpack.config.dev.js
# config/webpack.config.prod.js
# config/webpackDevServer.config.js
# config/env.js
# config/paths.js
# config/polyfills.js
```


# [yarn](https://yarnpkg.com/) package manager
## [installation](https://classic.yarnpkg.com/en/docs/install#debian-stable)
## [configuration](https://classic.yarnpkg.com/en/docs/cli/config/)
```sh
yarn config list
```

# NextJS
* [Next.js Documentation](https://nextjs.org/docs)
* [Learn Next.js](https://nextjs.org/learn)
* [the Next.js GitHub repository](https://github.com/vercel/next.js/)
```sh
npx create-next-app my-app
```
start nextjs
```sh
npm run-script build
npm run-script start
# or ( the same for debug )
node server.js
```
## nextjs in real environment
![nextjs-how-to](https://user-images.githubusercontent.com/8113355/210836797-7cd87530-5892-480d-8a1e-682350300239.png)

