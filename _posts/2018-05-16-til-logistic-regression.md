---
layout: post
title: "TIL: Logistic Regression"
author: "Lasse Schultebraucks"
categories: Machine Learning
comments: true
---

In the last few days I learned about Logistic Regression as one method to solve classification problems.

Classification problems are similar to [regression problems](https://lasseschultebraucks.com/machine/learning/2018/04/30/til-cost-functions-for-linear-regression.html), except that the values which will be predicted by our hypothesis will be discrete numbers. 
E.g. a binary classification problem only predicts values in which y can take $$ 0 $$ or $$ 1 $$. So there are just two classes to predict.
An example for binary classification would be predicting if an incoming email is spam or not spam. There would be two classes: One class of emails
which are spam ($$ y = 0 $$) and one class of emails which are not spam ($$ y = 1 $$). Another example for binary classification would be credit card fraud detection. There would also be two classes:
One class of transactions which are fraud ($$ y = 0 $$) and one class of transactions which are not fraud ($$ y = 1 $$).

Of course classification problems can also have more classes than two. One classification problem with three classes is the famous [iris data set](https://en.wikipedia.org/wiki/Iris_flower_data_set). The data set
consists of 150 samples of three different flowers. Every flower in the data set is described by four features (sepals and petals length and width).

One form of the hypothesis function $$ h_{\theta} (x) $$ for a binary classification problems would be as following:

$$ h_{\theta} (x) = g ({\theta}^T x) $$ where  $$ z = \theta ^T x $$ and $$ g(z) = \frac{1}{1+ \epsilon^{-z}} $$ 

This form of hypothesis for classification uses the sigmoid function $$ g(z) = \frac{1}{1+ \epsilon^{-z}} $$. It can there either be called sigmoid function or
logistic function. In the following I will use logistic function and logistic regression model to describe the hypothesis.

The Logistic function $$ g(z) $$ looks as following:

![](https://upload.wikimedia.org/wikipedia/commons/8/88/Logistic-curve.svg)

Because the function maps any real number to $$ (0,1) $$ it is very useful for defining a hypothesis for a binary classification problem.

Every $$ h_{\theta} (x) \geq 0.5 $$ will be classified as $$ y = 1 $$ and every $$ h_{\theta} (x) < 0.5 $$ will be interpreted as $$ y = 0 $$.

In the logistic function every inputs which is greater or equal to zero will be greater than or equals than 0.5 as output (like you can see above in the graph).

Therefore we can say that every $$ \theta^T x \geq 0 $$ will be classified as $$ y = 1 $$ and every  $$ \theta^T x < 0 $$ will be classified as $$ y = 0 $$.

A decision boundary is called the line which separates the are $$ y = 0 $$ and $$ y = 1 $$. The line will be directly created by the hypothesis function.

E.g. if $$ \theta = \begin{pmatrix} 3 \\ -1 \\  0 \\ \end{pmatrix} $$ then $$ y = 1 $$ if
$$ 3 + (-1)x_1 + 0x_2 \geq 0 $$ which results to $$ x_1 < 3 $$ . So the decision boundary will be a straight vertical line one the graph where $$ x_1 = 3 $$.
Everything on the left excluding the points on $$ x_1 = 3 $$ will be in the class $$ y = 1 $$ and everything on the right including the points on $$ x_1 = 3 $$ will be in the class of $$ y = 0 $$.

Logistic Regression does also work for non linear models through changing the shape of $$ \theta $$, e.g. through feature engineering.