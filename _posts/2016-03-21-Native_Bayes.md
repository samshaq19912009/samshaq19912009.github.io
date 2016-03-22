---
layout: post
title: Native Bayes notes
date: 2016-03-21
categories: blog
tags: [Machine Learning,Notes]
description: Native Bayes
use_math: true
---




#Native Bayes

In GDA, the feature vector x is continous

Here deals with discrete value

##Text Classification

model
$$ p(x|y) $$ 

if **vocabulry** has 5000 word, end up with $(2^{5000} - 1)$-dimensioanl parameters

**Assumption**: x is independet of y

$$ p(x_{1}.....x_{5000}|y) = \prod_{i=1}^{n} p(x_{i}|y) $$ 

**Making new predictions:**

$$p(y=1|x) = \frac {p(x|y=1)p(y=1)}{p(x)}$$

**Discretize**

##Laplace smoothing 

**Avoid zero in the denominator**

change 

$$ \phi_{j} = \frac {\sum_{i=1}^{m}1(z^{(i) }=j) }{m}  $$

into 

$$ \phi_{j} = \frac {\sum_{i=1}^{m}1 (z^{(i) }=j) + 1 }{m+k}  $$

## Event models for text classification ##




