---
layout: post
title: CNN 读书笔记（4） Netural Network 初入门(2)
date: 2016-08-01
categories: blog
tags: [Machine Learning,Notes, CNN]
description: CNN

---



## Data Preprocessing

Input vector: $X [ N * D ]$

**Mean subtraction**

```
X -= np.mean(X, axis = 0)
```

**Normalization**

```
(X /= np.std(X, axis = 0)
```

![](http://cs231n.github.io/assets/nn2/prepro1.jpeg)


**PCA and weighting**

> Compute the covariance matrix

```
# Assume input data matrix X of size [N x D]
X -= np.mean(X, axis = 0) # zero-center the data (important)
cov = np.dot(X.T, X) / X.shape[0] # get the data covariance matrix

```

> Perform singluar value decomposition

the columns of U are eigenvectors. S is the eigenvalues

```
U,S,V = np.linalg.svd(cov)

Xrot = np.dot(X, U) # decorrelate the data

Xrot_reduced = np.dot(X, U[:,:100]) # Xrot_reduced becomes [N x 100]


```
> Weightening


```
# whiten the data:
# divide by the eigenvalues (which are square roots of the singular values)
Xwhite = Xrot / np.sqrt(S + 1e-5)

```

![](http://cs231n.github.io/assets/nn2/prepro2.jpeg)



**In practise**

No PCA and weighten, only zero-centered the data

**Common Pitfall**

An important point to make about the preprocessing is that any preprocessing statistics (e.g. the data mean) must only be **computed on the training data**, and then applied to the validation / test data. E.g. computing the mean and subtracting it from every image across the entire dataset and then splitting the data into train/val/test splits would be a mistake. Instead, the mean must be computed **only over the training data** and then **subtracted equally from all splits** (train/val/test).


## Weight Initialization

**Pitfall: all zero initialization**:

There is no source of asymmetry between neurons if their weights are initialized to be the same.

**Small random numbers with variance calibraion**

```
w = np.random.randn(n) / sqrt(n)
```

Initution: Output $s = \sum_i^n w_i x_i$ has the same variance

$$
% <![CDATA[
\begin{align}
\text{Var}(s) &= \text{Var}(\sum_i^n w_ix_i) \\\\
&= \sum_i^n \text{Var}(w_ix_i) \\\\
&= \sum_i^n [E(w_i)]^2\text{Var}(x_i) + E[(x_i)]^2\text{Var}(w_i) + \text{Var}(x_i)\text{Var}(w_i) \\\\
&= \sum_i^n \text{Var}(x_i)\text{Var}(w_i) \\\\
&= \left( n \text{Var}(w) \right) \text{Var}(x)
\end{align} %]]>
$$

**In practise with Relu activation**

```
w = np.random.randn(n) * sqrt(2.0/n)
```

## Regularization

**L2 regularization**

**L1 regularization**

In practice, if you are not concerned with explicit feature selection, L2 regularization can be expected to give superior performance over L1.

**Drouout**


![](http://cs231n.github.io/assets/nn2/dropout.jpeg)

## Loss function

### **Classification**

**SVM Loss**

$L_i = \sum_{j\neq y_i} \max(0, f_j - f_{y_i} + 1)$

**Softmax Loss**

$L_i = -\log\left(\frac{e^{f_{y_i}}}{ \sum_j e^{f_j} }\right)$

**Cross-entropy Loss**

$L_i = \sum_j y_{ij} \log(\sigma(f_j)) + (1 - y_{ij}) \log(1 - \sigma(f_j))$

### Regression

**L2 norm**

$L_i = \Vert f - y_i \Vert_2^2$

**Caution**

> When faced with a regression task, first consider if it is absolutely necessary. Instead, have a strong preference to discretizing your outputs to bins and perform classification over them whenever possible.
