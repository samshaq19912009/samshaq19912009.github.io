---
layout: post
title: Independent Component Analysis
date: 2016-04-02
categories: blog
tags: [Machine Learning,Notes]
description: ICA
---

Image there is some data $s \in \mathbb{R}^n $ that is generated via n independent sources, What we observe is 
$$ x= As $$
where $A$ is an unknown square matrix called the **mixing matrix**.Repeated observations gives us a dataset $\{x^{(i)}; i=1,...m  \}$, and our goal is to recover the sources $s^{(i)}$.

Let $W = A^{-1}$  be the **unmixing matrix**, $w_i^T$ denote the i-th row of $W$, so that
$$
W = \begin{pmatrix}
w_1^T \\
.\\
.\\
.\\
w_n^T
\end{pmatrix} 
$$
Thus, $w_i \in \mathbb{R}^n$ and the j-th source can be recovered by computing $s_j^{(i)} = w_j^T x^{(i)}$

## ICA ambiguities

### Permuatation matrix

### Scaling problem

### Rotation (for Guassian)

## Densities and linear transformations

$$ p_x(x) = p_s(Wx) \cdot W $$

## ICA algorithm

We suppose that the distribution of each source $s_i$ is given by a density $p_s$, and that the joint distribution of the source is given by
$$p(s) = \prod_{i=1}^m p_s(s_i)$$
which gives the following density on $x = As = W^{-1}s$
$$p(x) = \prod_{i=1}^m p_s(w_i^T x) \cdot W$$
All that remains is to specify a density for the individual sources $p_s$

Recall that, given a real-valued random variable $z$, its cumulative distribtuion function(cdf) $F$ is defined by $F(z_0) = P(z \leq z_0) = \int_{-\infty}^{z_0}p_z(z) dz$. Also the density of $z$ can be found from the cdf by taking its derivative:$p_z(z) = F^{'}(z)$.

Using the **sigmoid function** $g(s) = 1/(1 + e^{-s})$, hence $p_s(s) = g^{'}(s)$.

The square matrix $W$ is the parameter in our model. Given a training set $\{x^{(i)}; i=1,...m  \}$, the log likelihood is given by 
$$l(W) = \sum_{i=1}^m (\sum_{j=1}^n \log g^{'}(w_j^Tx^{(i)}) + \log W) $$

We would like to maximize this in terms $W$. By taking derivatives and using the fact that $\nabla_W W = W (W^{-1})^T$, doing a stochastic gradient ascent learning rule. Now the update rule is 

$$W := W + \alpha \left( \begin{pmatrix}
1 - 2g(w_1^Tx^{(i)}) \\
1 - 2g(w_2^Tx^{(i)})\\
.\\
.\\
1 - 2g(w_n^Tx^{(i)})
\end{pmatrix} {x^{(i)}}^T + (W^T)^{-1} \right)
$$

After the algorithm converges, we then compute $s^{i} = W x^{(i)}$ to recover the original sources.

**Remark**. When writing down the likelihood of the data, we implicitly assumed that $x^{(i)}$ were independent of each other.

