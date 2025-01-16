---
title: Machine learning
date: 2024-05-29T05:31:06.066+05:30
draft: true
tags:
  - machine_learning
  - AI
---



Machine learning
- Supervised learning  -> Train data with lables
- Unsupervised learning -> FInd pattern in data and create a cluster
- Reinforcement learning ->based on feedback 

## Supervised Learning

#### **Regression**
Regression tasks involve predicting a continuous value for each input data point. Examples include predicting house prices based on features like square footage and number of bedrooms, predicting stock prices based on historical data.
- Ordinal Regression
- Linear Regression

#### **Classification**
In classification tasks, the algorithm predicts a categorical label or class for each input data point. Examples include spam detection (classifying emails as spam or not spam)
- Binary Classification
- Multi-class Classification

## UnSupervised Learning

perceptron








## Feature engineering
process of creating new features or transforming existing features in a dataset to improve the performance of machine learning models. It involves selecting, extracting, and transforming raw data into meaningful features that can help the model better understand the underlying patterns in the data.


## Model performance assessment metrics

**Confusion Matrix**: A confusion matrix is a table that is often used to describe the performance of a classification model on a set of test data for which the true values are known. It consists of four elements:

- True Positive (TP): The number of instances correctly predicted as positive.
- True Negative (TN): The number of instances correctly predicted as negative.
- False Positive (FP): Also known as Type I error, the number of instances incorrectly predicted as positive.
- False Negative (FN): Also known as Type II error, the number of instances incorrectly predicted as negative. A confusion matrix provides insights into the performance of a classification model and can be used to calculate various metrics such as accuracy, precision, recall, and F1-score.

**Accuracy**: Accuracy is the ratio of correctly predicted instances to the total number of instances in the dataset. It is calculated as: 
```
Accuracy= TP + TN / TP + TN +FP +FN​
```


**Cost-Sensitive Accuracy**: Cost-sensitive accuracy takes into account the costs associated with different types of errors. It assigns different weights or costs to different types of errors based on their importance. For example, in medical diagnosis, the cost of false negatives (missed diagnoses) might be much higher than the cost of false positives (incorrect diagnoses). Cost-sensitive accuracy is calculated by adjusting the weights of TP, TN, FP, and FN accordingly.


**Precision**: Precision is the ratio of correctly predicted positive instances to the total number of instances predicted as positive.
```
	Precision = TP / TP + FP
```

**Recall (Sensitivity)**: Recall, also known as sensitivity or true positive rate, is the ratio of correctly predicted positive instances to the total number of actual positive instances.

```
Recall=TP / TP + FN
```

**F1-Score**: F1-score is the harmonic mean of precision and recall. It balances precision and recall and provides a single metric that summarizes the performance of a classifier.

```
F1_score = 2* Precision * recall / Precision + recall 
```











## Resources
- [ML system design: 450 case studies to learn from](https://www.evidentlyai.com/ml-system-design)
- https://distill.pub/

Maths
- https://freedium.cfd/https://towardsdatascience.com/how-to-learn-math-for-machine-learning-fast-even-with-zero-math-background-159757833c3a