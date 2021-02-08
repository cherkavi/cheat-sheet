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

## [npm commands](https://docs.npmjs.com/cli-documentation/cli)

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
