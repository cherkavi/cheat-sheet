# installation
* [install NodeJS](https://github.com/nodejs/help/wiki/Installation)
* [install Angular](https://cli.angular.io/)

# npm
## check installation
```
npm bin -g
```

## print high level packages
```
npm list -g --depth=0
```

## reinstall package globally
```
# npm search @angular
npm uninstall -g @angular/cli
npm cache clear --force
npm install -g @angular/cli 
```

## build project ( install dependencies )
```
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
