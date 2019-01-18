---
layout: post
title: "Preparation of training data"
author: "Lasse Schultebraucks"
comments: true
---

This blog post is part of a series, where I talk about concepts and algorithms in Machine Learning. In this part I want to talk a little about the preparation of data before you start training the data to your Machine Learning algorithm.

### Basic feature engineering: Selecting the right features

In the feature engineering you typically care about your features and decide if they are relevant to your question you want to answer. Further you also create new features out of existing features as described later. For the most machine learning algorithms the data must be numerical, but there are also existing algorithms which can deal with categorical features. Normally you explore your data set and decide what features seems to be predictive of the target value. With these selected features you then train your selected machine learning algorithm. After your first pick of features you then can add other less obvious strong features which seem to be related to your feature. If the model then performs better, you can re do the step again and again until you find the best selection of features.

### The number of the perfect training data

There is of course no optimal answer to this question. But there are certain factors which determine the accuracy of your model, e.g. if you have less data your accuracy will be probably less then you will have with more training data. Further for more complex and non-linear patterns you also will need more training data to explore the full range of complexity in your traing data. Another huge factor of the number of training data you need is the dimension of your features. If you have 100 features you will probably need way more than 1000 entries in your data set, but if you only have 2 features 1000 entries maybe will be fine. In general you can say, that the more data there is, the higher will be the accuracy.

### Quality of the data

Quantify is important, but another huge factor is the quality and representativeness of the training data. If you want to detect if an online account is from a men or from a women, but your training set includes entries mostly from men, you probably will not have huge success at training it. Also if your training data only includes old data entries, but you want to predict new events the chance is also high of a lose of accuracy. So on the one hand side the training data has to be represantive for every possible target you want to determine and also must be up to data.

### Changing categorical features to numerical

The most machine learning algorithms are only working with numerical features, so you have to be sure that you prepare the training data right before training the model. Categorical features are features like gender and relationship status. One simple way to deal with this problem is to transform the categories into multiple binary features. If we take the example of gender, then we can transform the categorical feature gender to two binary features, male and female. If the entry has a 1 on male the entry refers to a man, if the entry has a 1 on female the entry refers to a women.

### Replacing missing data

In an ideal world there exists no missing data in a data set, but in the real world there exist missing data. So we have to deal with this problem, too. One naive way would be to just remove the whole entry if there is data missing in it, but then we remove possible essential data entries which later could helps us to improve the accuracy of our model. This approach only work if we have a large data set with only less data missing. Therefore we have to replace the missing data fields with fake data. If there are missing data on categorical features, you will most likely create a new category for missing value. If there is missing data on numerical features, we replace them by values which are outside of the real spectrum of the feature, so e.g. you could replace missing entries in the feature age with are negative number like -1. Sometimes this can work, but an more elegant way is the concept of imputation, where you replace missing data by guessing the true value. One approach for imputation is for numerical features is replacing missing data by the median.