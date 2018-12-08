---
layout: post
title: "Angular: Common Http Client"
author: "Lasse Schultebraucks"
---

With the last major release of Angular to Angular 4.3 there came a new feature to the Angular framework: The new Angular [HttpClient](https://angular.io/guide/http/). In the following blog post I want to show you how to use the new Angular HttpClient.



### Core features of the Angular HttpClient
So in case you do not know what a HttpClient is, it is simply a way to communicate with backend services over the HTTP protocol. HTTP stands for Hypertext Transfer Protocol and is e.g. one popular way to get a website out of the World Wide Web and displayed at your browser.

There are many features of the new Angular HttpClient:

- Strong typing of request and response objects
- Better error handling via APIs based on Observables
- Interceptors
- Testability support

### Basic setup
Lets see them in action. Therefore I have created a new Angular project with the Angular CLI. You can find all the code also on GitHub. First we have to import the new HttpClientModule from @angular/commom/http. You can still find the old Angular HttpClient in @angular/http. Also import the new Angular HttpClient under imports in the application module (app.module.ts).

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

// New HttpClientModule
import { HttpClientModule } from '@angular/common/http';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule // Do not forget importing!
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
Next we create an post interface which should represents our mock data later which we want to request (post.interface.ts).
```typescript
export interface Post {
  userId: number;
  id: number;
  title: string;
  body: string;
}
```
Before we are using the new client, lets modify our HTML template which should represent our model which we request later (app.component.ts).

```html
<div>
  <p>userId: {{post?.userId}}</p>
  <p>id: {{post?.id}}</p>
  <p>title: {{post?.title}}</p>
  <p>body: {{post?.body}}</p>
</div>
```
### Strong typing of request and response objects
Next we go to the app.component.ts and create a simple HTTP GET request. I use a [public API](https://jsonplaceholder.typicode.com/) here as an example, which provides mock data (app.component.ts).

```typescript
import { Component, OnInit } from '@angular/core';
import { HttpClient, HttpParams } from '@angular/common/http';

import { Post } from './post.interface';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {

  post: Post;

  constructor(private http: HttpClient) {
  }

  ngOnInit() {
    this.http.get<Post>('https://jsonplaceholder.typicode.com/posts/1', {observe: 'response'}).subscribe(response => {
        this.post = response.body;
        console.log(response);
      },
      (err) => {
        if (err.error instanceof Error) {
          console.log('An error occurred on the client side', err.error.message);
        } else {
          console.log(`Backend returned code ${err.status}, body was: ${err.error} (server error)`)
        }
      });
  }

}
```
Okay what are we doing here? First we importing the Angular HttpClient from `@angular/common/http`, the new location of the new Angular HttpClient. Remember, the old Angular HttpClient is still on `@angular/http`. Then we define our an instance of the Angular HttpClient in the constructor and in `ngOnInit()` we create a simple HTTP request.

But there is something special here about the HTTP request compared to requests he have done with the old client. We expect an Post object here, so that is the first feature we use with the HTTP Client. Typechecking makes sure that you are handling the data in the right way which you receive and makes your code much cleaner and easier to understand.

Also we can now receive the full response of the GET request. So if there are any important information hidden in e.g. the headers you want to know, you can check for them now.

Further we can do better error handling now. If the request returns and error of instance Error, we know that an error happened on the client side, else there is any error happened on server side.

With `.retry()` you can simply retry the request. So if there happened any error, you can just automatically recheck. `retry()` can be imported `from rxjs/add/operator/retry`.

```typescript
http .get<Post>('https://jsonplaceholder.typicode.com/posts/1')
     .retry(3)
    .subscribe(...);
```
Also if your api does not provide JSON data, you can receive non-JSON data by changing the response type like this:

```typescript
http
    .get('https://jsonplaceholder.typicode.com/posts/1', {responseType: 'text'})
    .subscribe(post => console.log(post));
```
### Interceptors
Interceptors are a big feature in the new Angular HttpClient. So what are interceptors doing? They are sitting between application and the backend and transforming requests which you make from client side and they are also transforming the responses from the backend in a way you want. This is especially useful for authentication if you have to authenticate as a user every time if you want to start a request and also for logging.

In the following example we create a simple interceptor for our post model, which id should be modified (post.interceptor.ts).

```typescript
import { Injectable } from '@angular/core';

import {
  HttpEvent, HttpInterceptor, HttpHandler, HttpRequest, HttpResponse } from '@angular/common/http'

import { Observable } from 'rxjs/Observable';
import 'rxjs/add/operator/do';

@Injectable()
export class PostInterceptor implements HttpInterceptor {
  intercept (req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    const postReq = req.clone({
      // Modify what you want
      headers: req.headers.set('Authorization', '123') // e.g. some auth token for access
    });
    return next.handle(postReq).do(event => {
      if (event instanceof HttpResponse) {
        const time = new Date().toLocaleString();
        console.log(`Request happened at ${time}.`); // Logging in console when request happened
      }
    })
  }
}
```
So first we import a few things from `@angular/common/http` and from `rxjs`. Then we write our `PostInterceptor` class, which implements `HttpInterceptor`. It has only one method: `intercept`. We can do multiple things here. First we can change the content of the request, so e.g. if we have to authenticate on every request, we can do that easily here. Also we can create a logging system here for every http request and response, without much effort.

But at the moment the interceptor does not work, we first have to declare it in our app.module.ts:

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

// New HttpClientModule
import { HTTP_INTERCEPTORS, HttpClientModule } from '@angular/common/http';

import { AppComponent } from './app.component';
import {PostInterceptor} from "./post.interceptor";

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    HttpClientModule // Do not forget importing!
  ],
  providers: [{
    provide: HTTP_INTERCEPTORS,
    useClass: PostInterceptor,
    multi: true
  }],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

We can provide multiple interceptors here, they will all apply one by one in order.

### Resume
There are many more features and things to explore about the Angular HttpClient, you can click [here](https://angular.io/guide/http) to find out more. The code from this blog post will be available on [GitHub](https://github.com/LSchultebraucks/angular-httpclient).
