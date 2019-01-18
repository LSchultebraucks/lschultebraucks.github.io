---
layout: post
title: "Gaussian Naive Bayes"
author: "Lasse Schultebraucks"
comments: true
---

So I currently learning some machine learning stuff and therefore I also exploring some interesting algorithms I want to share here. This time I want to talk about the Gaussian Naive Bayes algorithm, which is a simple classification algorithm which is based on the Bayes' theorem.

### Bayes' theorem

Bayes Theorem is named after Thomas Bayes (1701-1761), who first introduced Bayes' theorem which then later get developed further by Pierre Simon Laplace, who published the modern equation of the Bayes Theorem in 1812. In general Bayes Theorem describes the probability of an event, based on prior knowledge of conditions be related of conditions to the event. So it basically fits perfectly for machine learning, because that is exactly what machine learning does: making predictions for the future based on prior experience. Mathematically you can write the Bayes theorem as following:

![Bayes Theorem]({{site.url}}/assets/bayes_theorem.svg)

Let's break the equation down:

- A and B are events.
- P(A) and P(B) (P(B) not 0) are the probabilities of the event independent from each other.
- P(A\|B) is the probability A under the condition B.
- Equivalent with P(B\|A), it is the probability of observing event B given that event A is true.

So for me as a non expert in the probability theory P(A\|B) and P(B\|A) seemed first a little bit confusing for me. So this probabilities are also called conditional probability and they are describing the probability of A under the condition of B. So lets see that on an example: Lets say that that A stands for the probability, that if you look outside your house "you will see at least one person". Lets assume that B stands for "it is raining". Then P(A\|B) stands for the probability that you will see at least one person outside your house if it is raining. This of course works also for negations. So P(A\|not B), where not B stands for "it is not raining", will describe the probability that you will see at least one person outside your house if it is not raining.

Okay, back the the Bayes' theorem. Lets take an example and see how it works. I try to show you many examples, because I often find it myself difficult to understand certain topics, especially math topics if there are no concrete examples.

### Example

- Lets say that A is the event, that a person is ill, not A person is not ill.
- There exists a test, B stands for a positive test result, not B stands for a negative test result.
- The probability A is P(A) = 0.01 (1%), P(not A) = 0.99 (99%)
- The probability that the test is correct if A is ill is P(B\|A) = 0.99 (99%) and same that if the test is false the person is also not ill P(not B\|not A) = 0.99 (99%). This is also called that the test is 99% sensitive (positive test result and person is ill) and 99% specific (negative test result and person is not ill).

So with a probability of 50% a random person with a positive test result is ill. With other words: A random person with a positive test result is not ill in 50% of the cases.

Why is that so? It is simple: Because there are much more people who are actually not ill. Lets assume that we have 100 people. 99 people are not ill, 1 person is ill. 99 (not ill people) * 0.01 (probability of wrong test results) = 0.99. So there is 0.99 false positives expected. 1 ill person * 0.99 (probability of correct test results) = 0.99. That leads us to a probability of 0.5 aka a 50% of a random person with a positive test result.

So with Bayes' theorem you can calculate pretty easy the probability of an event based on the prior probabilities and conditions.

### Gaussian Naive Bayes

The Gaussian Naive Bayes is one classifier model. Beside the Gaussian Naive Bayes there are also existing the Multinomial naive Bayes and the Bernoulli naive Bayes. I picked the Gaussian Naive Bayes because it is the simplest and the most popular one.

### Iris data set

On of the most popular data sets in machine learning ist definitely the iris data set. The iris data set is about flowers who has three features: sepal length, sepal width and petal length and petal width. Each flower is labeled as one of three species: setosa, versiscolor and virginica. The data set has a total of 150 entries, so it is very small.

Lets see how it will perform with the Gaussian Naive Bayes classifier.

```python
#
# iris dataset
# 150 total entries
# features are: sepal length in cm, sepal width in cm,petal length in cm        - petal width in cm\n
# labels names: setosa, versicolor, virginica
#
# used algorithm: Gaussian Naive Bayes (GaussianNB)
#
# accuracy ~100%
#
from time import time
import numpy as np
from sklearn.datasets import load_iris
from sklearn.naive_bayes import GaussianNB
from sklearn.metrics import accuracy_score

def main():
	data_set = load_iris()
	
	features, labels = split_features_labels(data_set)
	
	train_features, train_labels, test_features, test_labels = split_train_test(features, labels, 0.18)
	
	print(len(train_features), " ", len(test_features))
	
	clf = GaussianNB()
	
	print("Start training...")
	tStart = time()
	clf.fit(train_features, train_labels)
	print("Training time: ", round(time()-tStart, 3), "s")
	
	print("Accuracy: ", accuracy_score(clf.predict(test_features), test_labels))

def split_train_test(features, labels, test_size):
	total_test_size = int(len(features) * test_size)
	np.random.seed(2)
	indices = np.random.permutation(len(features))
	train_features = features[indices[:-total_test_size]]
	train_labels = labels[indices[:-total_test_size]]
	test_features  = features[indices[-total_test_size:]]
	test_labels  = labels[indices[-total_test_size:]]
	return train_features, train_labels, test_features, test_labels

def split_features_labels(data_set):
	features = data_set.data
	labels = data_set.target
	return features, labels

if __name__ == "__main__":
	main()
```

The Jupyter Notebook and the Python file will be also available on GitHub. As we can see the classifier is very fast, even it is no big data set, and the accuracy is perfect. Of course with other permutations and other ratio between training and testing set there will be other results.

### Adult census income

Next lets see at another example. I found [this data set](https://www.kaggle.com/uciml/adult-census-income) which contains entries of people, features are specified in age, workclass, education, occupation, race and many more. Each entry is labeled which income. It is either below 50k or below. So the task is to train a classifier, e.g. a Gaussian Naive Bayes classifier, and find out for future entries if the peoples income is above or below 50k.

That is what I did:

```python
#
# https://www.kaggle.com/uciml/adult-census-income
# there are 32,562 total entries
# features are: "age","workclass","fnlwgt","education","education.num",
# "marital.status","occupation", "relationship","race","sex","capital.gain",
# "capital.loss","hours.per.week","native.country","income"
# label is "income"
#
# used algorithm: Gaussian Naive Bayes (GaussianNB)
#
# accuracy ~80%
#
import os
import pandas as pd
from time import time
from sklearn.naive_bayes import GaussianNB
from sklearn.metrics import accuracy_score
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder

def main():
	data_set = load_data_set()
	print(data_set.info())
	
	# data cleaning and preprocessing
	data_set = clean_data_set(data_set)
	
	train_set, test_set = train_test_split(data_set, test_size=0.2, random_state=3)
	print(len(train_set), " ", len(test_set))
	
	train_features, train_labels = split_features_labels(train_set, "income")
	test_features, test_labels = split_features_labels(test_set, "income")
	
	clf = GaussianNB()
	
	print("Start training...")
	tStart = time()
	clf.fit(train_features, train_labels)
	print("Training time: ", round(time()-tStart, 3), "s")
	
	print("Accuracy: ", accuracy_score(clf.predict(test_features), test_labels))

def load_data_set():
	csv_path = os.path.join("adult.csv")
	return pd.read_csv(csv_path)

def split_features_labels(data_set, feature):
	features = data_set.drop(feature, axis=1)
	labels = data_set[feature].copy()
	return features, labels

def clean_data_set(data_set):
	for column in data_set.columns:
	if data_set[column].dtype == type(object):
	le = LabelEncoder()
	data_set[column] = le.fit_transform(data_set[column])
	
	return data_set

if __name__ == "__main__":
	main()
```

The accuracy is here only 80% unfortunately, but with feature engineering and other algorithms the [accuracy will be up to 87%](https://www.kaggle.com/uciml/adult-census-income/feed).

### Resume

The Naive Bayes classifiers are working based on the Bayes' theorem, which describes the probability of an event, based on prior knowledge of conditions be related of conditions to the event. It is a very simple and fast classifier and works sometimes very good, and even without much effort you can get a okay accuracy.