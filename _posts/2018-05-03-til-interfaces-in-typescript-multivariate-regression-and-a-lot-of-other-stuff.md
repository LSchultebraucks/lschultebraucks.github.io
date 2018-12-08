---
layout: post
title: "TIL: Interfaces in TypeScript, Multivariate Regression and DFAs."
author: "Lasse Schultebraucks"
categories:  TIL
---

Today I learned (and yesterday) about Interfaces in TypeScript and how you can use them correctly to use them as your DTO in you application,
multivariate regression and a bunch of other topics from my university courses. A lot about DFA, e.g. how to minimize them and how to build the product out of two DFAs.

There was a lot of troubleshooting in the last two days because of my internet connection. Now I have to switch the router and hope that then my internet connection will come back.
Until then I will just have a connection through a hotspot which is hosted on my mobile phone...

Back to the topic: In the past I have used normal classes in TypeScript for dealing with objects, especially DTOs. Yesterday I learned that Interfaces are a much better solution for dealing
with DTOs in Typescript. You just declare the attributes in the Interface and create an object of the type from the interface with following notation:

<script src="https://gist.github.com/LSchultebraucks/73511fd1411ad220f7b060ac5591dcc5.js"></script>

Next topic: Multivariate regression, which is basically the same as [linear regression with one variable](https://lasseschultebraucks.com/machine/learning/2018/04/30/til-cost-functions-for-linear-regression.html), 
but except with multiple variables. I will probably write more about them in the next days.

Last but not least I learned a lot in university too, mainly more advanced topics about DFAs like minimizing or creating the product out of two DFAs.