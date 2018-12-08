---
layout: post
title: "TIL: Classifying unbalanced data classes in Machine Learning"
author: "Lasse Schultebraucks"
categories:  Machine Learning
---

With this blog post I introduce a new post category on my blog: *Today I Learned* or short *TIL*. 
In a recent post I wrote about that I have less time for writing this blog currently and therefore I do not want to concentrate on writing long blog posts like in the past.
Therefore I will try out *TIL* and post new things that I have learned about in a couple of short paragraphs on my blog. The intention for this is to deepen the learned, to archive it and to share it.
I probably won't blog on a daily basis.

Today I learned about how to deal with unbalanced classification problems, e.g. if you have a binary classification problem and 95% of you instances belong to class A and the other 5% to class B.
If you train a model with these ratio, you will likely have a high accuracy, but the model would classify nearly all class B incorrectly. Examples for this problem are [credit card fraud detection](https://www.kaggle.com/mlg-ulb/creditcardfraud/) and detection of rare diseases.

The easiest way is of course just to collect more data. This would solve the problem and you would go on. But this is most of the time not applicable.

Another solution is changing the performance metric. Because accuracy is a very misleading metric in this regard as explained above, we could choose another metric like a [confusion matrix](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.confusion_matrix.html), [precision](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html), [recall](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html) or [F1 score](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html).

One more way of solving this problem is resampling the dataset. You can either [upsample or downsample](https://en.wikipedia.org/wiki/Oversampling_and_undersampling_in_data_analysis) the dataset to have more balanced classes in your dataset.
If you upsample, you add copies of instance of the under-represented class in your dataset. Opposite you delete instances of the over-represented class to apply downsampling.

There is a great example of classifying imbalanced classes for a credit card fraud detection problem on [Kaggle](https://www.kaggle.com/joparga3/in-depth-skewed-data-classif-93-recall-acc-now).