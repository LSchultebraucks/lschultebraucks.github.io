---
layout: post
title: "TIL: Cost functions for linear regression"
author: "Lasse Schultebraucks"
categories:  Machine Learning
---

Today I learned about cost functions, which are also known as mean square error (MSE), for linear regression problems.

I just started the [Machine Learning Course](https://www.coursera.org/learn/machine-learning/home/welcome) on Coursera by Andrew Ng to
get a better understanding of how Machine Learning works and extends my knowledge in the field.

To recap: Linear regression is a way to model a relationship between *X* and *y*. 
There is also multivariate linear regression where there are multiple *X*s to predict a *y*. Linear regression with one variable can be described as following:

![](https://latex.codecogs.com/gif.latex?h_%5Ctheta%28x%29%20%3D%20%5Ctheta_0%20&plus;%20%5Ctheta_i%20x)

To measure the accuracy of the hypothesis above, there can be use a cost function, which takes the average difference of all the hypothesis results and where *m* is the number of training examples.

![](https://latex.codecogs.com/gif.latex?J%28%5Ctheta_0%2C%20%5Ctheta_1%29%20%3D%20%5Cfrac%7B1%7D%7B2m%7D%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%28h_%5Ctheta%28x_%7Bi%7D%29-y_%7Bi%7D%29%29%5E2)

In other words: It is the mean of the difference between the predicted value and the actual value of the hypothesis. Another name for the cost function is also
mean squared error (MSE).

For the hypothesis we have to chose ![](https://latex.codecogs.com/gif.latex?%5Ctheta_0) and ![](https://latex.codecogs.com/gif.latex?%5Ctheta_1). Then we can check the hypothesis with the cost function above.
The result of the function is always non-negative and values which are closes to zero are better, because it supports the hypothesis. The goal is to minimize ![](https://latex.codecogs.com/gif.latex?J%28%5Ctheta_0%2C%20%5Ctheta_%29)
to build an optimal hypothesis.

An example to make things clear: There is an existing training set with the values *(1/1)*, *(2/3)* and *(3/5)*. We choose -1 for ![](https://latex.codecogs.com/gif.latex?%5Ctheta_0) and 2 for ![](https://latex.codecogs.com/gif.latex?%5Ctheta_1).
Maybe you already can imagine this graph, and the linear function and therefore you know that the values for ![](https://latex.codecogs.com/gif.latex?%5Ctheta_0) and ![](https://latex.codecogs.com/gif.latex?%5Ctheta_1)
are pretty accurate.

![](https://latex.codecogs.com/gif.latex?%5Cfrac%7B1%7D%7B2*3%7D%5Csum_%7Bi%3D1%7D%5E%7B3%7D%28-3&plus;3x_i-y_i%29%20%3D%20%5Cfrac%7B1%7D%7B2*3%7D*%28%28-1&plus;2&plus;1-2%29%5E2&plus;%28-1&plus;2*2-3%29%5E2&plus;%28-1&plus;2*3-5%29%5E2%29%20%3D%20%5Cfrac%7B1%7D%7B2*3%7D*%280%5E2&plus;0%5E2&plus;0%5E2%29%20%3D%20%5Cfrac%7B1%7D%7B2*3%7D*0%20%3D%200)

Perfect! For the 3 values above ![](https://latex.codecogs.com/gif.latex?%5Ctheta_0) and ![](https://latex.codecogs.com/gif.latex?%5Ctheta_1) are optimal.
