---
layout: post
title: "Dealing with CORS in Angular (development)" 
author: "Lasse Schultebraucks"
categories:  Angular
comments: true
---

Cross-origin ressource sharing (CORS) is a mechanism used in HTTP to prevent browser and webclients requesting from another domains. Some requests may  work, others may not. The purpose of CORS is to determine if it is safe to communicate in regards of server and client.

 A couple of days ago I build a web client with Angular and used a REST API which backend I launched local. So I both launched in local development, both in different ports of localhost. To enable proper communication between server and client, I had to use a proxy which does the  requests at the backend API instead of the client.
 
I first created a file named `proxy.conf.json` in the root of the Angular CLI project.

```conf
{
  "/api/*": {
    "target": "http://localhost:3000",
    "secure": false,
    "logLevel": "debug",
    "changeOrigin": true
  }
}
```

This will enable all request make to `/api..` forwarded to `http://localhost:3000`. So instead of making a request directly to `http://localhost:3000/api/..` we doing are requests to `http://localhost:4200/api/..` which request will be redirect to `http://localhost:3000`. This will allow us to bypass any possible CORS security problem in local development.

Next we  have to do some changes in our `package.json` to enable the `proxy.conf.json` when we run `npm start`. 

```json
"scripts": {
    "ng": "ng",
    "start": "ng serve --proxy-config proxy.conf.json",
    "build": "ng build --prod",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e"
  }
```

However, this solution is not for production, but does work fine for local development.