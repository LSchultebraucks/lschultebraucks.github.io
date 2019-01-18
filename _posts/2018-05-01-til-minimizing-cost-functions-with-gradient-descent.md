---
layout: post
title: "TIL: Minimizing cost functions with gradient descent"
author: "Lasse Schultebraucks"
categories:  Machine Learning
comments: true
---

Today I learned about gradient descent and how you can minimize cost function for liner regression problems with gradient descent.

In [yesterdays TIL](https://lasseschultebraucks.com/machine/learning/2018/04/30/til-cost-functions-for-linear-regression.html) 
I wrote about linear regression and cost functions (also known as MSE) for measuring the accuracy of a hypothesis in linear regression problems.

So what is a gradient descent? With gradient descent you can minimize a cost function by finding the local minimum.
Essentially gradient descent figures out which ![](https://latex.codecogs.com/gif.latex?%5Ctheta)s we have to choose for optimizing our hypothesis.
The gradient descent algorithms is as following:

*repeat until convergence:*

![](https://latex.codecogs.com/gif.latex?%5Ctheta_j%20%3A%3D%20%5Ctheta_j%20-%20%5Calpha%20%5Cfrac%7B%5Cdelta%7D%7B%5Cdelta%20%5Ctheta_j%7D%20J%28%5Ctheta_0%2C%5Ctheta_1%29)

*J = 0,1* representing the feature index numbers. For each iteration every ![](https://latex.codecogs.com/gif.latex?%5Ctheta) should update simultaneously.

![](https://latex.codecogs.com/gif.latex?%5Calpha) represents the learning rate. If the learning rate is to large, gradient descent can overshoot the minimum and might not able to find it.
If the learning rate is to small, gradient descent can be slow. 

![](https://latex.codecogs.com/gif.latex?%5Cfrac%7B%5Cdelta%7D%7B%5Cdelta%20%5Ctheta_j%7D%20J%28%5Ctheta_0%2C%5Ctheta_1%29) is our [derivative](https://en.wikipedia.org/wiki/Derivative).

The following figure shows an example of gradient descent. The *x* and *z* axis are ![](https://latex.codecogs.com/gif.latex?%5Ctheta)s and the *y* axis is the value of our cost function *J* of our hypothesis *h*.
With each iteration our hypothesis changes and we approximate at the local minimum. 

![]({{site.url}}/assets/gradient_descent_method.png)