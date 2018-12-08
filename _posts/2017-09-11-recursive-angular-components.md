---
layout: post
title: "Recursive Angular components"
author: "Lasse Schultebraucks"
---

Recursion is a well known concept in Software Development and is a useful tool when it comes to algorithms which use e.g. divide an conquer to solve a problem recursively. But beside standard algorithms you can also use them in Angular components to build recursive components. It's actually really simple and useful.

### Why you should build recursive components?

Well, if you have tree data structures or lists in lists in lists you are coming to your limit with classic ngFor's. Much easier and cleaner is it when your reuse your component over and over again by recursion, because then your code is nice and clean. Lets see and easy example how you can do it!

### A simple example

First we need a recursive data structure. Therfore I have created a simple one with two attributes. A string and a list of References to another Objects of the same type:
```typescript
// node.model.ts
export class Node {
  public header: string;
  public nodes: Node[]; // recursive data structure
}
```

I also have declared some mock data in my root component:

```typescript
// app.components.ts
import { Component } from '@angular/core';

import { Node } from './model/node.model';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  node: Node = {
    header: '1. I am the root!',
    nodes: [
      { header: '1.1. We are all inner nodes',
        nodes: [
          { header: '1.1.1. A',
            nodes: []
          },
          {
            header: '1.1.2. B',
            nodes: []
          },
          {
            header: '1.1.3. C',
            nodes: []
          }
        ]
      },
      { header: '2.1. We are all inner nodes',
        nodes: [
          { header: '2.1.1. A2',
            nodes: []
          },
          {
            header: '2.1.2. B2',
            nodes: []
          },
          {
            header: '2.1.3. Leafs here',
            nodes: []
          }
        ]
      },
      { header: '3.1. We are all inner nodes',
        nodes: [
          { header: '3.1.1. A3',
            nodes: []
          },
          {
            header: '3.1.2. Last but not least',
            nodes: []
          }
        ]
      }
    ]
  };
}
```

Next I generate a new component which will be my recursive component. This is the template:

```html
<!-- recursion.component.html -->
<div>
  <ul>
    <li>
      {{node.header}}
    </li>
    <div *ngIf="!isLeaf(node)">
      <ul *ngFor="let innerNode of node.nodes">
        <app-recursion [node]="innerNode"></app-recursion>
      </ul>
    </div>
  </ul>
</div>
```

This is my typescript file of component:

```typescript
// recursion.component.ts
import { Component, Input, OnInit } from '@angular/core';

import { Node } from '../model/node.model';

@Component({
  selector: 'app-recursion',
  templateUrl: './recursion.component.html',
  styleUrls: ['./recursion.component.css']
})
export class RecursionComponent implements OnInit {
  @Input() node: Node;

  constructor() { }

  ngOnInit() {
  }

  private isLeaf(): boolean {
    return this.node.nodes.length === 0;
  }
}
```

I have written a `isLeaf` method which is not necessary, but if you build more complex templates, where will be eventually place reserved after the ngFor it is nice to know and use to characteristics of tree. But in this case it is not necessary, should only be for demonstration purposes.

With no much effort we could build a tree like view. You can check out the code [here](https://github.com/LSchultebraucks/angular-recursion-example).

### Resume

Recursive components in Angular are a great way to reuse components and to build easy components for more complex data structures like trees.