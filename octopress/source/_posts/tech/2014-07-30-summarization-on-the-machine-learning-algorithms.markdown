---
layout: post_tech
title:  "Summarization on the Machine Learning Algorithms"
date:   2014-07-30 12:00:00 +0800
comments: true
categories: [tech]
tags: [python, algorithm, machine learning]
---

Table of Contents

- 1\. Classification
- 2\. Prediction
- 3\. Unsupervised Learning
- 4\. Dimensionality Reduction
- 5\. Big Data and Map Reduce

## 1. Classification

| Algorithm  | Maths                   | Data     | Pros       | Cons        |
|:-----------|:------------------------|:---------|:-----------|:------------|
| kNN        | euclidean distance: sqrt{sum(xi-xj)^2} | Numeric values, nominal values | High accuracy, insensitive to outliers, no assumptions about data | Computationally expensive, requires a lot of memory |
| Decision Tree | shannon entropy: - p(x) * log(p(x)) | Numeric values, nominal values | Computationally cheap to use, easy for humans to understand learned results, missing values OK, can deal with irrelevant features | Prone to overfitting |
| Naive Bayes | bayes, ln(a * b) = lna + lnb, lnf(x)~ f(x) | Nominal values | Works with a small amount of data, handles multiple classes | Sensitive to how the input data is prepared |
| Logistic Regression | sigmoid: 1/(1+e^-z), gradient ascent/descent: w = w +/- alpha * delta(f(w)) | Numeric values, nominal values | Computationally inexpensive, easy to implement, knowledge representation easy to interpret | Prone to underfitting, may have low accuracy |
| Support Vector Machine | hyperplane: arg max{min (label * W^T * x+b) * 1/\|W\|}, SMO algorithm (Sequential Minimal Optimization): finds alpha (increased/decreased) and b, alpha criteria: outside margin boundary, not already clamped/bounded | Numeric values, nominal values | Low generalization error, computationally inexpensive, easy to interpret results | Sensitive to tuning parameters and kernel choice; natively only handles binary classification |
| AdaBoost Meta-Algorithm | boosting/bagging: (boosting) weighted sum of all classifiers, error = # of misclassified / # of examples, alpha = 1/2 * ln (1 - error / error), weightedVector = probability * exp(-1 * alpha * categoryMatrix * classifiedMatrix) | Numeric values, nominal values | Low generalization error, easy to code, works with most classifiers, no parameters to adjust | Sensitive to outliers |  


## 2. Prediction

| Algorithm  | Maths                   | Data     | Pros       | Cons        |
|:-----------|:------------------------|:---------|:-----------|:------------|
| Linear Regression | *  | Numeric values, nominal values | Easy to interpret results, computationally inexpensive | Poorly models nonlinear data |
| Tree-based Regression | bestFeatureSplit: targetMean, targetVariance | Numeric values, nominal values | Fits complex, nonlinear data | Difficult to interpret results |


\* (Linear Regression) Maths

- regression - (X^T * X) inversed
    - Y = X^T * W
    - W = (X^T * X)^(-1) * X^T * Y

- locally weighted linear regression (LWLR) - (X^T * X) inversed
    - yHat = (X^T * W * X)^(-1) * X^T * W * y
    - (kernel) W = exp (\|xi - x\|/(-2k^2)), constant k - how much to weight nearby points

- ridge regression - (X^T * X) NOT inversed
    - Y = X^T * X + lambda * I
    - W = (X^T * X + lambda * I)^(-1) * X^T * y

- forward stagewise regression - greedy algorithm
    - lasso


## 3. Unsupervised Learning


| Algorithm  | Maths                   | Data     | Pros       | Cons        |
|:-----------|:------------------------|:---------|:-----------|:------------|
| Unlabeled items grouping: k-means clustering | mean, euclidean distance: (x-y)^2, squared error: distnace^2 | Numeric values | Easy to implement |  Can converge at local minima; slow on very large datasets |
| Association analysis: apriori | probability, confidence | Numeric values, nominal values | Easy to code up | May be slow on large datasets |
| Frequent itemsets finding: FP-growth | frequency count, tree (data structure) | Nominal values | Usually faster than Apriori | Difficult to implement; certain datasets degrade the performance |


## 4. Dimensionality Reduction

 
| Algorithm  | Maths                   | Data     | Pros       | Cons        |
|:-----------|:------------------------|:---------|:-----------|:------------|
| Principal Component Analysis (PCA) | covariance, eigenValues, eigenVectors | Numerical values | Reduces complexity of data, indentifies most important features | May not be needed, could throw away useful information |
| Singular Value Decomposition (SVD) | [DATA]m*n = [U]m*m [SIGMA]m*n [V^T]n*n | Numeric values | Simplifies data, removes noise, may improve algorithm results | Transformed data may be difficult to understand |


## 5. Big Data and Map Reduce


| Algorithm  | Maths                   | Data     | Pros       | Cons        |
|:-----------|:------------------------|:---------|:-----------|:------------|
| MapReduce  | | Numeric values, nominal values | Processes a massive job in a short period of time | Algorithms must be rewritten; requires understanding of systems engineering |


MapReduce: a framework for distributed computing

### 5.1. Machine Learning Algorithms for MapReduce


| Algorithm                          | MapReduce                                                                   |
|:-----------------------------------|:----------------------------------------------------------------------------|
| Naive Bayes                        | mapper - results of the probability of a feature, reducer - sum up results  |
| k-Nearest Neighbors (kNN)          | tree: to narrow search for cloest vectors                                   |
| Support Vector Machines (SVM)      | Pegasos algorithm: stochastic gradient descent                              |
| Singular Value Decomposition (SVD) | Lanczos algorithm: find the singular values in a large matrix               |
| k-means Clustering                 | Canopy clustering                                                           |


