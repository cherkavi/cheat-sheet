# installation
* [install NodeJS](https://github.com/nodejs/help/wiki/Installation)
* [install Angular](https://cli.angular.io/)

# npm
## proxy
```sh
npm config ls
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

## check installation
```sh
npm bin -g
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

## print high level packages
```sh
npm list -g --depth=0
```

## reinstall package globally
```sh
# npm search @angular
npm uninstall -g @angular/cli
npm cache clear --force
npm install -g @angular/cli 
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
PORT=$PORT npm start --prefix $PROJECT_HOME/api
```


# [yarn](https://yarnpkg.com/) package manager
## [installation](https://classic.yarnpkg.com/en/docs/install#debian-stable)
## [configuration](https://classic.yarnpkg.com/en/docs/cli/config/)
```sh
yarn config list
```
