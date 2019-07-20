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

