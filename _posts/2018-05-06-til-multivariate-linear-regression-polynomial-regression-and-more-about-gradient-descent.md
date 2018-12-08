---
layout: post
title: "TIL: Multivariate Linear Regression, Polynomial Regression and more about Gradient Descent"
author: "Lasse Schultebraucks"
categories: Machine Learning
---

Today I learned about Multivariate Linear Regression, Polynomial Regression and more about Gradient Descent.

I also created a Python script which let me create the layout for posts, especially TIL posts much faster.

As I mentioned in a post a couple of days ago I learned more about multivariate Linear Regression. So Linear Regression
with multiple features. The hypothesis function with multiple features looks as following:

$$ h_{\theta_0} (x)= \theta_0 + \theta_1x_1 + + \theta_2x_2 + ... + \theta_nx_n $$

By using the definitions of matrix multiplication, the multivariate hypothesis function can als be written as following:

$$ h_ {\theta_0} (x) = \begin{pmatrix}\theta_0 && \theta_1 && \dots && \theta_n \end{pmatrix} \begin{pmatrix}x_0 \\\ x_1 \\\ \vdots \\\ x_n\end{pmatrix} = \theta^T x $$

where $ x_0^{(i)} $ is defined as $ 1 $ ($ \theta_0 $ works just as a constant).

The form of gradient descent for multivariate linear regression looks as following:

$$ \text{repeat until convergence: \{} \\ \theta_j := \theta_j - \alpha \frac{1}{m}\sum_{i=1}^m(h_{\theta}(x^{(i)})-y^{(i)})*x_j^{(i)} \\ \text{\}} $$

$ x_0^{(i)} $ is again defined as $ 1 $. The rest basically works much [like with one variable](https://lasseschultebraucks.com/machine/learning/2018/04/30/til-cost-functions-for-linear-regression.html).

Some words about Feature Scaling and Learning Rate. Feature scaling is important, because it speeds up the gradient descent.
$ \theta $ basically descend quicker on smaller ranges than on larger rangers. Ideally all variables look like $ -1 <= x_{(i)} <= 1 $.

A feature can be scaled with following formula:

$$ x_i := \frac{x_i - \mu_i}{s_i} $$

where $ \mu_i $ is the average of all the value for feature $ i $ and $ s_i $ is the range of values $ (max-min) $. Example: If $ x_i $ is a feature with range of values from 10 to 100 and a mean of 55, then

$$ x_i := \frac{feature - 55}{90} $$

Again some words to the Learning Rate $ \alpha $. It is important that $ \alpha $ is not to large, else $ J(\theta) $ may not decrease and not converge. On the other side, if $ \alpha $ is too small, $ J(\theta) $ will converge very slowly.

Last but not least some more words to Polynomial Regression: The hypothesis does not be a linear function, it can be also a polynomial function, e.g. a quadratic function:
$ h_\{\theta_0} (x) = \theta_0 + \theta_1 x_1 + \theta_2 x_2^2 $. I will probably write more about Polynomial Regression in the next days. Just wanted to note them here.
