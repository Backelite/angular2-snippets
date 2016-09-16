# Angular 2 Snippets

`Updated on 2016, Sept 16` | `Version: 2.0`


## TypeScript

```
var num: number = 5;
var name: string = "Speros"
var something: any = 123;
var list: Array<number> = [1,2,3];

function square(num: number): number {
	return num * num;
}

class Person {
   constructor(public name: string){

   }
}
```

## Components

### Inline Template & Style

`./app.component.ts`
```
@Component({
   selector: 'my-app',
   template: `<h3>Task List Application</h3>`,
   styles: ['h3 { color: gray; }']
})
export class MyAppComponent {}
```

### Remote Template & Style

`./app.component.ts`
```
@Component({
   selector: 'my-app',
   templateUrl: 'app.component.html',
   styleUrls: ['app.component.css']
})
export class MyAppComponent {}
```

## Directives

### Attributes Directives

```
<span [class.red]="true"></span>
<span [ngClass]= "{ red: true, box: true }"></span>
<span [ngStyle]="{ background-color: blue, color: black }"></span>
<span [customOnHoverHighlight]></span>
```

### Structural Directives

```
<h4 *ng-If="imTrue">This will show<h4>

<li *ng-For="let task of tasks">
   {{ task }}
</li>

```

## Data Flow

### Interpolation

```
<span>{{ myNumber }}</span>
<input [value]="myNumber">
```

### Event Binding

`./app.component.ts`
```
@Component({
   selector: 'my-app',
   template: `
   <h3>Task List Application</h3>
   <button (click)="doThis()"></button>
   `
})
export class MyAppComponent {
   doThis(){
    	console.log("You clicked on the button");
   }
}
```

### Two Way Binding

```
<input [value]="num"
(keyup)="num = $event.target.value" >

<input [(ngModel)]="num">
```

## Providers

### Services

`./services.task.service.ts`
```
import { Injectable } from '@angular/core';

@Injectable()
export class TaskService{
   tasks = ["First Task", "Second Task", "Third Task"];

   getTasks(){
	return this.tasks;
   }

   addTask(task){
 	this.tasks.push(task);
   }
}
```

`./app.component.ts`
```
import { Component } from '@angular/core';
import { TaskService } from './services/task.service';

@Component({
    providers: [ TaskService ]
})
export class MyAppComponent {
    tasks: string[];
    constructor(private taskService:TaskService) {
        this.tasks = this.taskService.getTasks();
    }
}
```
