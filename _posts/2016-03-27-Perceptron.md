
---
layout: post
title: Perceptron
date: 2016-03-23
categories: blog
tags: [Machine Learning,Notes]
description: Perceptron
---

## 1. The perceptron and large margin classifiers

**Online learning**: algorithm has to make predictions continuouly even while it's learning


the perceptron algorithm has parameters $\theta \in  R^{n+1}$, and makes its predictions according to

$$h_{\theta}(x) = g(\theta^{T}x)$$

Where 

$$g(z) = 1 \quad \mbox{if} z  \geq 0  $$
$$g(z) = -1 \quad  \mbox{if} z \leq 0  $$


**Theorem (Block and Novikoff)**
Let a sequence of examples $(x^{(1)},y^{(1)})$, $(x^{(2)},y^{(2)})$,.... $(x^{(m)},y^{(m)})$, suppose that $||x^{(i)}|| \leq D$ for all i, and further that there exists a unit-length vector $u$ such that $y^{(i)} \cdot (u^{T}x^{(i)}) \geq \gamma$ for all examples in the sequence ( i.e. $ u^{T}x^{(i)} \geq \gamma $ if $y^{(i)} = 1$, $ u^{T}x^{(i)} \leq \gamma $ if $y^{(i)} = -1$), so that u separates the data with a margin at least $\gamma$, Then the total number of mistakes that percepton algorithm makes on this sequence is at most $(D/\gamma)^2$

**Proof** The perceptron updates its weigths only on those examples on which making mistakes. Let $\theta^{(k)}$ be the k-th mistake, which implies 

$$(x^{(i)})^T \theta^{(k)} y^{(i)} \leq 0 $$

Also form the perceptron learning rule, we would have that $\theta^{(k+1)} = \theta^{(k)} + y^{i}x^{i}$. 

$$(\theta^{(k+1)})^T u = (\theta^{(k)})^T u + y^{i}(x^{i})^T u
\geq (\theta^{(k)})^T u + \gamma$$

By a straightforward inductive argument, implies that 

$$ (\theta^{(k+1)})^T u  \geq k\gamma$$

Similarly, we could get

$$ ||\theta^{(k+1)}||^2 \leq kD^2   $$
 
then it is clear that $k \leq (D/\gamma)^2$ 

Q.E.D




