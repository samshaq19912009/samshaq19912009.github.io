---
layout: post
title: CNN 读书笔记（5） Convolutional Neutral Network
date: 2016-08-03
categories: blog
tags: [Notes, CNN]
description: CNN

---

## The convolution layer:

* Accept an input layer $W_1 * H_1 * D_1$

* Require following paramters:

	* Number of filters K

	* their spatial extent F

	* Stride size S

	* Amount of zero padding P

* Produce a volume of size $W_2 * H_2 * D_2$

	* $W_2 = (W_1 -F + 2*P) / S + 1$

	* $H_2 = (H_1 -F + 2*P) / S + 1$

	* $D_2 = K$

* With paramter sharing, it introduces a $F \cdot F \cdot D1 $ weights per filter, for a total of $F \cdot F \cdot D1 \cdot K$ weights and $K$ bias 

* In the output layer, the d-th depth slice ( of size $W2 \cdot H2$) is the result of performing a valid convolution of the d-th filter over the input volume with a stride $S$, and then offset by d-th bias


## The Pooling layer:

* Accept an input layer $W_1 * H_1 * D_1$

* Require following paramters:



	* their spatial extent F

	* Stride size S


* Produce a volume of size $W_2 * H_2 * D_2$

	* $W_2 = (W_1 -F) / S + 1$

	* $H_2 = (H_1 -F) / S + 1$

	* $D_2 = K$

* Introduce **zero-paramter** since it computes a fixed function 

* No zero-padding for Pooling layers

