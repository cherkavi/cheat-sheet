# installation
## install nodejs
  ```sh
  # download and unpack to $destionation folder https://nodejs.org/en/
  destination_folder=/home/soft/node2
  wget -O node.tar.xz https://nodejs.org/dist/v10.16.3/node-v10.16.3-linux-x64.tar.xz
  tar -xf node.tar.xz -C $destination_folder
  # update /etc/environment with $destination_folder
  ```
## npm config
> $HOME/.npmrc - another way to extend settings per user
  ```sh
  # list of configuration
  npm config list
  # full config list with default settings
  npm config ls -l
  # set proxy 
  npm config set proxy http://<username>:<pass>@proxyhost:<port>
  npm config set https-proxy http://<uname>:<pass>@proxyhost:<port>
  ```
  
## docker container with Angular attach to your current folder and build your application
```
+---------+
| source  +-----------+
|  /app   |           |
+----^----+    +------v----+
     V         | docker:   |
     |         | * node    |
     |         | * angular |
 +---+---+     +-----+-----+
 | dest  |           |
 | build <-----------+
 +-------+
```
  1. build your docker with dependencies
```sh
NODE_VERSION=16.15.0
    # NODE_VERSION=latest # you can select last version for it
docker pull node:$NODE_VERSION 
docker run --entrypoint="" --rm --name "npm_angular" --interactive --tty node:$NODE_VERSION  /bin/sh 

    # (optional)
    # ------- inside container -------
    ## install angular 
npm install -g @angular/cli    
npm install -g @angular/cli@12
    # check your installation 
ng --version

    ## install typescript
npm install -g typescript  
tsc --version

    ## install yarn
npm install --global yarn
yarn --version
```
  2. (optional) save your docker container with installed artifacts
```sh
DOCKER_IMAGE_ANGULAR=node-with-angular
# in another terminal 
CONTAINER_ID=`docker ps | grep 'npm_angular' | awk '{print $1}'`
echo $CONTAINER_ID
docker commit $CONTAINER_ID $DOCKER_IMAGE_ANGULAR
docker images
```
  3. start saved container in your application folder (folder with )
```sh
DOCKER_IMAGE_ANGULAR=node-with-angular
docker run --entrypoint="" --interactive --tty -p 4200:4200  -v `pwd`:/app $DOCKER_IMAGE_ANGULAR  /bin/sh 
```
  4. build application inside your container 
```sh
    # after step 3.
PATH_TO_PROJECT_LOCAL=/app
    # ls -la $PATH_TO_PROJECT_LOCAL/package.json
cd $PATH_TO_PROJECT_LOCAL
sudo rm -rf node_modules

npm install
    # /usr/local/lib/node_modules/npm/bin/npm 
    # npm build # for version<6.x.x
npm pack
```
 
# Start of application
* [angular-cli](https://github.com/angular/angular-cli)
* [angular how to start with cli](https://angular.io/start)

# core conceptions
* modular architecture
  * Angular architecture patterns
  * Scalable Angular application architecture
* one way data-flow
  * Angular data flow best practices
  * Uni-directional flow in Angular
  * Advantages of one way binding
* directives
  * Angular attribute directives
  * Angular structural directives
  * Angular structural directive patterns
* components lifecycle
  * Angular life cycle hook
  * Component life cycle
* http services
  * JavaScript observable patterns
  * Angular HTTP and observables
  * ES7 observable feature
* smart/dumb components
  * Smart/dumb Angular components
  * Stateless dumb components
  * Presentational components
  * Smart components in Angular
* application structure
  * Single repo Angular apps
  * Angular libraries
  * Angular packages
  * Angular bundles
  * Angular micro apps
  * Monorepo
* property binding
  * Angular property binding 
  * Angular event binding 
  * Angular two-way binding
  * Angular interpolation
  * Angular passing constants 
* feature modules
  * Angular feature modules
  * Shared feature structures in Angular
  * Feature module providers
  * Lazy loading with routing and feature modules
* forms
  * Angular form validation
  * Template driven validation
  * Reactive form validation
  * Sync and async validators in Angular
  * Built-in validators 
  * Angular custom validators
  * Cross-field validation
* projection
  * Angular content projection
  * Angular parent-child view relationship
  * Angular view data relationships
* onPush
  * Angular onPush change detection
* route access restrictions
  * Angular route guards
  * Angular authentication patterns
  * Angular preloading and lazy-loading modules
  * Angular secured route patterns
* Angular custom pipes
* decorators
  * Angular decorators 
  * Viewchild and contentchild in Angular
  * Angular component data sharing
  * Angular directives patterns 
  * @Host, @HostBinding and exportAs in Angular 
* dynamic components
  * Dynamic components in Angular
  * Dynamic components and ng-templating
* manage state of application
  * Angular RxJs
  * Flux/Redux principles
  * Angular state management principles
* Dependency injection
  * Angular zones
  * Angular DI

# angular cli
## create new project
```
ng new my-new-project
```
## start a project
```
cd my-new-project
# open in VisualCode just generated project ```code .```
# start locally 
ng serve
# start on specific port
ng serve --port 2222
# start and open browser
ng serve --open
```

## build a project
```
ng build
ng build --prod
ng build --prod --base-href http://your-url
```

## [create component](https://angular.io/cli/generate#class-command)
```
ng g component my-new-component
ng generate component my-new-component
```

## create service
* generate by cli
  ```bash
  ng generate service myService
  ```
* create dummy data "src/app/my-service.service.ts"
  ```typescript
  data=[9,8,7,6,5]
  ```
* update "src/app/app.module.ts"
  ```typescript
  import { MyServiceService } from './my-service.service';
  ...
  providers: [MyServiceService],
  ```
using it in model "src/app/my-component/my-component.component.ts"
  ```typescript
  import { MyServiceService } from '../my-service.service';
  ...
  template: `
    <div>{{ this.externalService.data }}</div>
    <div>{{ this.mydata }}</div>
  `,
  ...
  export class MyComponentComponent implements OnInit{
  mydata:number[]
    ngOnInit(): void {
      this.mydata = this.externalService.data
    }
    constructor(private externalService:MyServiceService){}
  }
  ```


# angular templates
## inline template
```typescript
@Component({
  selector: 'app-my-component',
  // templateUrl: './my-component.component.html',
  template: `
  <b>my-component</b> <br/>
  `,
  styleUrls: ['./my-component.component.css']
})
```

## for loop
```typescript
@Component({
  selector: 'app-my-component',
  template: `
  <b>my-component</b> <br/>
  <i>is working inline ->{{description.title+"   "+description.values}}<- </i>
  <ul>
    <li *ngFor="let each of description.values; let index = index">{{ index }} {{ each }}</li>
  </ul>
  `,
  styleUrls: ['./my-component.component.css']
})

export class MyComponentComponent {
  description:object

  constructor() { 
    this.description={
      title: "my custom properties",
      values: [5,7,9,11,13]
    }
    
  }
}

```
## alternative template
```
@Component({
  selector: 'app-my-component',
  template: `
  <div *ngIf="description.customTemplate==true; else myAnotherTemplate">{{ description.values}}</div>  

  <ng-template #myAnotherTemplate>
    <ul><li *ngFor="let each of description.values"> {{ each }} </li></ul>
  </ng-template>
  `,
  styleUrls: ['./my-component.component.css']
})

export class MyComponentComponent {
  description:object
  constructor() { 
    this.description={
      title: "my custom properties",
      customTemplate: false,
      values: [5,7,9,11,13]      
    }    
  }

}
```

# Property binding

Component --data--> View

```typescript
<img src="{{ myProperty }}" >

<img [src]="myProperty" >
<button [disabled]="myProperty=='not-active-now'" >

<img bind-src="myProperty" >
```

# [Events](https://developer.mozilla.org/en-US/docs/Web/Events) binding

View --event--> Component 

```typescript
@Component({
  selector: 'app-my-component',
  template: `
    <button (click)="myEvent($event)">click event</button>
  `,
  styleUrls: ['./my-component.component.css']
})

export class MyComponentComponent {
  myEvent(event:MouseEvent){
    console.log(event)
    window.alert(event)
  }
}
```

# Styles
## inline style
```
@Component({
  selector: 'app-my-component',
  template: `
    <button>my button</button>
  `,
  styles: [`
   button {
     font-weight: bold;
     color: red;
   }
  `]
})
```

# Animation
## installation
```
 npm install @angular/animations@latest --save
```
## component 
```
import { trigger, state, style, transition, animate, keyframes } from '@angular/animations'

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
  animations: [
    trigger('myOwnAnimation',
            [
            state('small', style({transform: 'scale(1)'})),
            state('bigger', style({transform: 'scale(2)'})),
            transition(
              'small <=> bigger', 
              animate('300ms', style({transform: 'translateY(100px)'}))
            )
            ]
    )
  ]
})
export class AppComponent {
  state: string = "small"
  animateMe(){
    this.state = (this.state === 'small'? 'bigger' : "small")
  }
}

```

## template
```app.component.html
    <p [@myOwnAnimation]='state'  (click)="animateMe()" style="text-align: center"> animation</p>
```

# Test
## execute single test
```
ng test --include='**/service/*.spec.ts'
npm run test -- --include='**/service/*.spec.ts'
```

# UX tools
* Axure (Prototyping)
* Sketch (UI Design)
* Adobe Illustrator
