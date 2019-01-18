---
layout: post
title: "Introduction to Authentication and Authorization"
author: "Lasse Schultebraucks"
categories: [Security]
comments: true
---

Lately I have dealt a lot of with authentication and authorization. Therefore I will probably blog a little bit more about these topics in the next weeks. But before that happens I want to talk about specific technologies and tools I want to talk a little bit about the basics.

First let's understand what authentication and authorization is.

# Authentication

Authentication is the process of verifying that a claimed property is true. In the context of IT a machine or a person
claims to be someone, e.g. some entity and proves someone, e.g. a server where he wants to fetch some resources from, that
he is actually the one he claims to be. In the process of authentication not only the claim but also the confirmation
of the claim is involved. So if the confirmation has successfully happened, the authentication has been successful, else
the authentication failed because the person who claims to own an identity failed to prove it.

Every time you log in to a website you use authentication to prove the website server that you are actually the person
you claims to be. Without a secure authentication everybody could just claim to be someone else.

# Authorization

After the process of authentication has successfully happened, an authorization process decides which resources
you can access. So authorization is basically about distributing roles, rights and privileges and making sure that nobody can see or modify resources which are out of their scope.

Therefore authorization makes sure that just you - and the admins of a website - can make changes to your posted content. Just imagine that everybody could modify content on the web after a successful authentication process. There would be more than just chaos.
Also limiting the reading of resources is very important. Not everybody should see private shared content or sensitive data.

# Conclusion

Authentication and Authorization is very important for securing resources in the whole IT.

That's for now. In the next post I want to talk more about different authentication methods and Identity Provider.