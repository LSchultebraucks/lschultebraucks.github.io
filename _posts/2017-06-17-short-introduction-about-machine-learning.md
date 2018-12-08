---
layout: post
title: "Short Introduction about Machine Learning"
author: "Lasse Schultebraucks"
categories: [Machine Learning,  Python]
---

In this blog post, I want to give a short introduction about machine learning. I want to answer the questions what machine learning is, what problems machine learning faces and what machine learning algorithms are trying to solve. This will be just theoretical and should give a short introduction to the topic. I will talk about the definition of machine learning, different categories and types of machine learning problems and algorithms and last but not least about the challenges you face, if you want to develop a machine learning algorithm. I hope you will have fun and learn something new!

### Definition

So before we dive deeper into the topic, lets make clear what machine learning really is and what it is trying to solve. Machine Learning is a subfield in Computer Science, which has often more to do with statistics and analytics than Artificial Intelligence.

There is existing a favorite quote about what machine Learning is from Tom M. Mitchell. He said:

> "A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P if its performance at tasks in T, as measured by P, improves with experience E."

So machine learning algorithms learn from data and use their experience based on what they have learned to predict events of new data, which the algorithms have not dealt with.

So e.g. Spotify uses machine learning to predict what songs the user likes or not. Also these days house market values are often determined by machine learning systems.

### Categories and different types of Machine Learning

The are multiple different categorizations and important concepts you can divide machine learning system into it, we will look at these multiple points:

- Supervised learning
- Unsupervised learning
- Reinforcement learning
- Batch learning
- Online learning
- Instance based learning
- Model based learning

So lets start and begin to clarify the first term.

### Supervised Learning

Supervised learning is called, when your training data - that is the data set you fed the algorithm with - includes a desired solution, also called label. This a typical classification tasks: You have data set with labels and your train your algorithm with it, so if there is any new data you algorithm can determine the label of the new data. Examples of supervised learning are:

- Spam filters, where user have flagged spam emails and the spam filter detects new emails as spam based on the user experience
- Handwriting recognition
- Any type of pattern or speech detection

### Unsupervised learning

Unsupervised learning is the opposite of supervised learning. You have a data set without labels, so without a desired solution for each data piece and you have find connection and correlation between the data. This is the case if you have just a huge data set and you have to deal with them. Examples are:

- Analytics, e.g. about the visitor at an E-Commerce shop or blog
- Sorting e.g. photo without knowing what is on the photo
- There also exist semi supervised learning, with if obviously a mixture of supervised and unsupervised learning. Some data pieces are labeled and some are not.

### Reinforcement learning

Reinforcement learning is a much more dynamic approach, where the algorithm communicates with an dynamic environment and learn so about dangers and rewards. The algorithm has a specific goal, which he tries to achieve, so the so called agent takes action and tries to reach the goal. While making different action based decision he learns what is good and what is bad form him and so he figures out what he has to do and what he has to avoid the reach his goal.

After knowing some different types of how the system learn with his data, lets look how the system can get his data.

### Batch learning

There are two general types of how you can fed your algorithm with your available data. At bath learning, you have all your data available and train with a huge data set your algorithm. You do this typical offline. Depending on how big your data set is, the computing process could take a lot of time and resources. Then if you launch your system, it will not learn with the new data pieces it get, but you can collect your new data pieces and with your available and new data you can train your system again. Because computing the system with the whole data could take a long time, you will probably update your system less than if you would do it with online learning. 

### Online learning

At online learning your train your system sequentiell by feeding it with new data - called mini batches. Because the learning with the mini batches is fast and cheap, the system updates often and continuous, this is ideal for data you receive regularly like stock prices. 

There is existing an important parameter in online learning, which should determine how fast your algorithm should adapt the mini batches. A to high learning rate is bad, because if your rapidly adapt the new data you system will forgot the old data fast, but if the learning rate is to low, it is also bad, because then your algorithm will learn probably to slow. So the big challenge in online learning is to monitor your system closely, so your system will learn in a acceptable speed and don't forgot too much about the old data.

### Instance based learning

There are two basic ways how you can train your machine learning algorithm to make predictions.

First there is instance based learning, where the machine learning algorithm compare characteristics of already labeled data to the new data and based on this, he made predictions.

### Model based learning

Then there is model based learning, a more common approach. You build a model and train it with your training data and then you make predictions with new data through the algorithm.

### Challenges in Machine Learning

If you deal with huge data sets and want to build a machine learning algorithm, you have to take care of two main aspects: Bad data and a bad algorithm. First you have to check the quality and quantity of your available data, because without a good data set you can not build a good machine learning algorithm which should make prediction about new data pieces. Second, you have to be sure that you fed your machine learning algorithm with your data the right way. Lets break these two points more down.

### Insufficient quantity of training data or too less data
If just do not have enough data, you machine learning algorithm will probably not fully learn of the data, so his prediction he will make on new cases will be not as sure as it would be if the machine learning algorithm would have had more data available. Simple problems need thousand of example and complex one, e.g. image or speech recognition need often time millions.

### Poor quality data
It does not help the machine learning algorithm, if there are million of data pieces, but the data is full of errors or important data feature are missing. So you have to either filter out the poor quality data pieces completely or you have to deal with data, where features aka a characteristics are missing in a certain way.

### Irrelevant features
Certain feature of data pieces are sometimes just not important for the solution of the problem. So if you detect that there is something which has completely no influence to the result of the solution, you better make your machine learning algorithm more simple and remove the feature completely.

### Generalizing
If the weight of the data is not representative, it can be happen, that you create and overgeneralizing algorithm. This is of course bad, because how we humans overgeneralize about other, we do not want that our machine learning algorithm pulls the wrong conclusion about new data. One solution to this problem is to simplify the model by reselecting the features.

### How to split your data: Testing and Validating
Before you train your machine learning algorithm, you want to split the data into two sets: A training set, which is typical above 80% of the available data, and a testing set, which is typical above 20% of the data. With the training set your train your model. Afterwards your validate your solution and determine your error rate. If the error rate is low, good, you probably have find a good machine learning algorithm. If not, than go back and fine tune your model, train again, and test your computed system with the testing set again until you have reached a lower error rate.

### Summary
Machine learning algorithm are trying to make predictions about new cases. They have learned with huge chunks of data and can solve various problems, which you would not solve with normal programming. The are multiple categories and types of machine learning algorithm like supervised and unsupervised learning, batch and online learning or model based or instance based learning. You mainly get challenged by bad data and/or a bad algorithm. And before you train your algorithm, you have to split your available data, so you can validate afterwards your solution.

I hope you have learned something about machine learning.