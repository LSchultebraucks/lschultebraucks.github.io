---
layout: post
title: "Basic Authentication" 
author: "Lasse Schultebraucks"
categories:  [Rest, Security]
comments: true
---

In one of my latest blog posts I talked about [authentication and authorization](https://lasseschultebraucks.com/security/2019/01/18/introduction-to-authentication-and-authorization.html).

Now I want to dive deeper and tackle common authentication methods for REST APIs.
In this blog post I start with one of the most common and easiest to implement authentication methods.

# Basic Authentication (BA)

One of the most common and easiest ways to secure a REST API is by using Basic authentication. Its a protocol which current standard is written down in [RFC 7617](https://tools.ietf.org/html/rfc7617) in 2015.

Technical you use the HTTP headers to send information in the form of

````Authorization: Basic <credentials````

where credentials is base64 encoded username (or id) and password joined by a colon.

So e.g. a User with username `user` and password `user` is represented in the HTTP header in the following way:

```Authorization: Basic dXNlcjp1c2Vy```

## Security

Basic Authentication is not confidential, which means that the information can be viewed by everyone and is not private. Because Base64 can simply be encoded and decoded it is not secured information like hashed or encrypted information. 

Nevertheless Basic Authentication can be used in a confidential way by simply using HTTPS instead of HTTP.

## Mechanism

Because of the technical conditions the browser is required to cache the the credentials of the user. This is necessary, because the headers has to be sent within every HTTP request. 

Therefore the server has no direct way to logout a user of his session. The server can't just declare the credentials of the user as expired or invalid. But instead he can redirect the client to a URL of the site where the credentials are incorrect by purpose. Another possibility is to call a JavaScript method to clear stored credentials. But this is dependent from browser. But probably the most and easiest way to create a log out mechanism is to send the user a 401 if the client clicked on log out and then direct him wrong credentials like a blank user and password. In this way his previous credentials will be deleted from cache and he is successfully unauthorized.

## Implementation

The actual implementation of Basic Authentication on client and server side is easy. 

Plain JavaScript actually provides with `window.btoa` a method to encode a string in base64. To save the credentials you can user the local storage in your browser.

On server side you can also just decode the credentials and match them with the actual password. If you are using frameworks like Spring Boot you can write your whole Security configuration at one place. The server side code implementation of Basic Authentication is probably more costly than the client side implementation, but this also heavily depends on your chosen technologies for your backend.

# Conclusion

Basic Authentication is an easy and fast way to implement authentication for your API and or web application. Be sure to always use HTTPS if you are in production and you are using Basic Authentication. SSL certificates are free therefore there is no excuse not to have one for your web application.