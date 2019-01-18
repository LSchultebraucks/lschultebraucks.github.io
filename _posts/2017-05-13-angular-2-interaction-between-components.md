---
layout: post
title: "Angular 2: Interaction between components"
author: "Lasse Schultebraucks"
comments: true


In Angular 2 there are different way to communicate between components. If you use Angular 2 regularly it is very important to know multiple possibilities to do this and also when you use which way to get an interaction between your components. Therefore I write about three possibilities in this blog post and also explain what are the advantages/disadvantages of them and when you should use which solution to build a clean and simple Angular 2 application. We will discuss about:

- Passing data from parent to child with input binding
- Passing data from child to parent with output binding
- Communication between components via a service

### Passing data from parent to children with input binding

Input binding is a very easy and clean solution if you just want to give data from you parent to you child component. Lets look at one example.

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `
    <p>{{data}}</p>
    <p>{{dataExtra}}.</p>
  `
})
export class ChildComponent {
  @Input() data: string;
  @Input('moreData') dataExtra: string;
}
```

In the `ChildComponent` we have two properties, `data` and `dataExtra`, both string, which we let display in our template with string interpolation. The property we get from our parent child and we put them with the `Input()` decorations in properties. Lets look at our parent component:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `
    <app-child
      [data]="data"
      [moreData]="moreData">
    </app-child>
  `
})
export class ParentComponent {
  data = 'Very important information down here';
  moreData = 'The headline is very descriptive';
}
```

There we also have two strings, which we bind in our template to our child component. We can either bind it directly to the variable name of the child varible or we can give it a key, which we can use in our ChildComponent to bind it to a local variable.

Of course the cleaner version is here the first approach. Keys are always more cumbersome and harder to deal with, but there also could be situation where keys are the better solution, e.g. if you want to build a more reusable component.

You also could use a intercept input property changes with a setter.

### Passing data from child to parent with output binding

As an opposite of input binding with output binding you can can send data from you child component to your parent component. To do this, we use the `@Output` decorations. Our child component will then look like this:

```typescript
import {Component, EventEmitter, Output} from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<button (click)="setData()">Set Data</button>
  `
})
export class ChildComponent {
  @Output() dataOutput: EventEmitter<string> = new EventEmitter<string>();
  data = 'Hello from the other side!';

  setData() {
    this.dataOutput.emit(this.data);
  }
}
```
In our child component we have a string now. If the buttons get clicked it triggers `setData()`, who will emit the string with a EventEmitter. In our parent component we will then know, that something has happened.

```typescript
import {Component, Output} from '@angular/core';

@Component({
  selector: 'app-parent',
  template: `
    <app-child
      (dataOutput)="receiveData($event)">
    </app-child>
    {{data}}
  `
})
export class ParentComponent {
  data: string;

  receiveData(message: string) {
    this.data = message;
  }
}
```

In our parent component we receive the event and get the value from the string out of it. Again, with string interpolation we show it to screen.

Thus we can say, that if you want to push data from your child component up to your parent component, this solution is a very clean and easy way to do this.

### Communication between components via a service

Last but not least there are services. With services you can build more complex communication networks between your components, but still having a relatively clean way to do this instead of building deep stacks of input and output decorations. So lets look a one example of a service:

```typescript
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs/Subject';
import { Observable } from 'rxjs/Observable';

@Injectable()
export class ParentService {

  private data: string;
  private subject: Subject<string> = new Subject();

  getData(): Observable<string> {
    return this.subject.asObservable();
  }

  setData(data: string) {
    this.data = data;
    this.subject.next(data);
  }

}
```

In our Service we use Subject and Observable from `rxjs`. There reason behind is clear, we want to give the component an observable, so the component can subscribe to it. Lets look how our child component have changed: 

```typescript
import {Component, } from '@angular/core';
import {ParentService} from "../parent.service";

@Component({
  selector: 'app-child',
  template: `<button (click)="setData()">Set Data</button>
  `
})
export class ChildComponent {
  data = 'Hello from the other side!';

  constructor(private parentService: ParentService) { }

  setData() {
    this.parentService.setData(this.data);
  }
}
```

Everything seems to look much more cleaner now, we just declare an instance of the ParentService in the constructor and use the service to set the data in our `setData()` function. You may wonder, that we are not providing the service anywhere. Well, that we will do in our `ParentComponent`:

```typescript
import {Component, OnInit } from '@angular/core';
import {ParentService} from "./parent.service";

@Component({
  selector: 'app-parent',
  template: `
    <app-child>
    </app-child>
    {{data}}
  `,
  providers: [ParentService]
})
export class ParentComponent implements OnInit {
  data: string;

  constructor(private parentService: ParentService) { }

  ngOnInit() {
    this.parentService.getData().subscribe(
    (data: string) => {
      this.data = data;
    });
  }
}
```

As said above, we provide here our service, so we can use it to subscribe to the data.

The reason behind why we just provide our service at one place is, that if we provide the service at multiple locations we create multiple instances of our service. And that is not what we want, we just want to have one service, so we can create a smooth communication between the components. So how to we know, when and where we have to declare our service? Well, it is actually pretty simple: If we want to create a global service, we declare our service in our app.module.ts file in providers. There we will list all our global services. But if we just want to use a service for a child/childs components and his parent, we declare it like in the parent component. This works, because if a component declares a service, it look first at itself if he provides the service, and if not, he go up the hierarchical structure of the components and looks in the next parent component if there the service is provided. 

Services can be used, if normal input and output properties do not perform well enough (based on the code purity and not on the performance).

### Other communication possibilities

There are also other possibilities like the use of `@ViewChild` or `ngOnChanges()` to interact between components.

You can read about them and also about all the other solution in the Angular 2 documentation, but after reading this blog post you should have now a basic understanding about the basic ways to interact between components and also when you should use which solution in which situation. 