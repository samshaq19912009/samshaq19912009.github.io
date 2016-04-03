---
layout: post
title: Principal Component Analysis
date: 2016-04-01
categories: blog
tags: [Machine Learning,Notes]
description: PCA
---

First, pre-process the data to normalize its mean and variance as follows:

1. Let $\mu = \frac{1}{m}\sum_{i=1}^m x^{(i)}$
2. Replace each $x^{(i)}$ with $x^{(i)} - \mu$
3. Let $\sigma_j^2 =\frac{1}{m}\sum_{i=1}^m (x^{(i)}_j)^2 $
4. Replace each $x^{(i)}_j$ with $x^{(i)}_j/\sigma_j$


Compute the **major axis of variation**:

Given a unit vector u and a point x, the length of the projection of x onto u is given by $x^Tu$, so we would like to choose a unit-length u so as to maximize:
$$\frac{1}{m}\sum_{i=1}^m ((x^{(i)})^Tu)^2 =  \frac{1}{m}\sum_{i=1}^m u^T x^{(i)} {x^{(i)}}^Tu = u^T (\frac{1}{m}\sum_{i=1}^m  x^{(i)} {x^{(i)}}^T) u  $$

So the problem subject to $u_2 = 1$ gives the principal eigenvector of $\Sigma = \frac{1}{m}\sum_{i=1}^m  x^{(i)} {x^{(i)}}^T $,which is just the empirical covariance matrix of the data

Then, to represent $x^{(i)}$ in this basis, we need only to compute the corresponding vector 
$$
y^{(i)} = \begin{pmatrix}
u_1^T x^{(i)} \\
u_2^T x^{(i)} \\
....\\
u_k^T x^{(i)}
\end{pmatrix} \in \mathbb{R}^k
$$
Thus, whereas $x^{(i)} \in \mathbb{R}^n $, the vector$y^{(i)}$ now gives a lower k-dimensional representation. PCA is therefore also refered as a **dimensinality reduction** algorithm.