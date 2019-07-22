# installation
[install Angular](https://cli.angular.io/)

# links
[quick start](https://github.com/angular/quickstart.git)
[angular installation](https://cli.angular.io)

# angular cli
## create new project
```
ng new my-new-project
```
## start a project
```
cd my-new-project
ng serve
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