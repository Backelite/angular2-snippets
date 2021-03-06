# Angular 2 Snippets

`Updated on 2016, Sept 16` | `Angular Version: 2.0`


## TypeScript

```
var num: number = 5;
var name: string = "Speros"
var something: any = 123;
var list: Array<number> = [1,2,3];

function square(num: number): number {
	return num * num;
}

interface Person {
    name: string;
    age?: number;
}

class Student:Person {
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

### Access self & child component

`./child.component.ts`
```
import { Component, ElementRef } from '@angular/core';

@Component({
   selector: 'child-component',
   template: '<p>Child</p>'
})
export class ChildComponent {
    constructor(elementRef: ElementRef) {
        this.elementRef = elementRef;
    }

    hide() {
        this.elementRef.nativeElement.classList.add('hide');
    }
}
```

`./app.component.ts`
```
import { Component, ViewChild, ElementRef } from '@angular/core';
import { ChildComponent } './app.component.ts'

@Component({
   selector: 'my-app',
   template: '<div><child-component #childComponent></child-component></div>'
})
export class MyAppComponent {
    @ViewChild('childComponent', { read: ElementRef }) childComponent: ElementRef;

    constructor() {
        this.childComponent.hide();
    }
}
```

### Conditionnally Add Attribute

`./customInput.component.ts`
```
@Component({
    selector: 'custom-input',
    inputs: ['required', 'pattern'],
    template: `
    <div class="custom-input">
        <input type="text" name="inputValue" [(ngModel)]="inputValue" [required]="required ? '' : null" [pattern]="pattern ? pattern : null"/>
    </div>
    `
})
export class CustomInputComponent {}
```

`./ap.component.html`
```
<custom-input required></custom-input>
<custom-input pattern="[0-9]{14}" ></custom-input>
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

## Routing

### Access Route Parameters

`./app.component.ts`
```
import { Component } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({})
export class MyAppComponent {
    private _sub: any;

    constructor(private _route: ActivatedRoute) {
    }

	ngOnInit() {
        this._sub = this._route.params.subscribe(
            (params: any) => {
                let email = params['email'];
                let token = params['token'];
            });
    }

	ngOnDestroy() {
        if(this._sub) {
            this._sub.unsubscribe();
        }
    }
}
```

### Access URL Query Parmeters

`./app.component.ts`
```
import { Component } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({})
export class MyAppComponent {
    private _sub: any;

    constructor(private _route: ActivatedRoute) {
    }

	ngOnInit() {
        this._sub = this._route.queryParams.subscribe(
            (params: any) => {
                let email = params['email'];
                let token = params['token'];
            });
    }

	ngOnDestroy() {
        if(this._sub) {
            this._sub.unsubscribe();
        }
    }
}
```

### Access URL Fragments (after a #)

`./app.component.ts`
```
import { Component } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({})
export class MyAppComponent {
    private _sub: any;

    constructor(private _route: ActivatedRoute) {
    }

	ngOnInit() {
        this._sub = this._route.fragment.subscribe(
            (param: any) => {
                let anchor = param;
            });
    }

	ngOnDestroy() {
        if(this._sub) {
            this._sub.unsubscribe();
        }
    }
}
```

## Misc

### Custom Typings

`./myservice.service.ts`
```
/// <reference path="./myExternalLib.d.ts" />
```

`./myExternalLib.d.ts`
```
declare class MyExternalLib {
}

declare namespace MyExternalLib {
}

declare module "MyExternalLib" {
	export = MyExternalLib;
}
```
