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

## build project
```
npm install
npm start
```

# angular
## links
[quick start](https://github.com/angular/quickstart.git)
[angular installation](https://cli.angular.io)

## create new project
```
ng new my-new-project
```
## start a project
```
cd my-new-project
ng serve
```

## create component
```
ng g component my-new-component
ng generate component my-new-component
```
