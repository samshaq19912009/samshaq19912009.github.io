---
layout: post
title: Learning Theory
date: 2016-03-23
categories: blog
tags: [Machine Learning,Notes]
description: Learning Theory
---

**Lemma**   **(The union bound)**

$$
P(A_{1} \cup ....\cup A_{k}) \leq P(A_{1}) +...+ P(A_{k})
$$

**Lemma**   **(The Hoeffding inequality)**
Let $Z_{1}$,....,$Z_{m}$ be m independent and identically distributed random variables drawn from a Bernouli($\phi$) distribution, Let $\hat\phi = (1/m)\sum_{i=1}^{m} Z_{i} $ and let any $\gamma > 0$ be fixed. Then

$$P(|\phi - \hat\phi| > \gamma < 2\exp(-2\gamma^2m) )$$



**Training error** aka **Empirical risk Empirical error**

$$ \hat\epsilon (h) = \frac{1}{m}\sum_{i=1}^{m} { {h(x^{i}) \neq y} } $$


**Generalization error**

if we draw a new sample, the probability of misclassify

$$ \epsilon (h) = P_{(x,y) \sim D} (h(x) \neq y ) $$


**Empirical risk minimization(ERM)**

Define the **hypothesis class H**, EMR can be regarded as
$$\hat h = arg min_{h \in H} \hat\epsilon (h)$$




### The case of finite H ###

Sample $(x,y) \sim D$

Set $Z = 1 \{ h_{i}(x) \neq y \}$

$Z_{j} = 1 \{ h_{i}(x^{j}) \neq y^{j} \}$

Now the training error would be the expected value of $Z$ and $Z_{j}$

$$ \hat\epsilon (h_{i}) = \frac{1}{m}\sum_{i=1}^{m} { Z_{j} } $$

$ \hat\epsilon (h_{i}) $ is exactly the mean of the m random variables $Z_{j}$ are drawn iid from a Bernoulli distribtuion with the mean $ \epsilon (h_{i}) $ 

$$P(|\hat\epsilon (h_{i})- \epsilon (h_{i}) | > \gamma ) < 2\exp(-2\gamma^2m) )$$


Now want to prove this will be applied to be true for all $ h \in H$

let $A_{i}$ denote $ P(|\hat\epsilon (h_{i})- \epsilon (h_{i}) | > \gamma $

Using the union bound:

$$P(\exists h \in H, \hat\epsilon (h_{i})- \epsilon (h_{i}) \geq \gamma ) \leq \sum_{i=1}^{k} 2\exp(-2\gamma^2m) $$


Now the conclusion

$$P(\neg \exists h \in H, \hat\epsilon (h_{i})- \epsilon (h_{i}) \geq \gamma ) \geq 1 - 2k\exp(-2\gamma^2m) $$

Given $\gamma$ and some $\delta$ how much $m$ must be before we can guarantee that with probability at least $1- \delta$ tranining error will be within $\gamma$ of generalization error

Setting $\delta = 2k\exp(-2\gamma^2m) $

 $$ m \geq \frac{1}{2\gamma^2}log\frac{2k}{\delta}$$


The traininng set size $m$ that a certain method or algorithm requires in order to achieve a certain level of performance is called **sample complexity**

The number of training example is only **logarithmic** in k


Similarly, hold $m$ and $\delta$ fixed and solve for $\gamma$ in the previous equation, within probability $1-\delta$ we have that for all $h \in H$

$$ P(|\hat\epsilon (h_{i})- \epsilon (h_{i}) | \leq \sqrt{\frac{1}{m}log\frac{2k}{\delta}}$$

Define $h^{*}$ is the best possible hypothesis


**Theorem** let any $m$, $\delta$ be fixed, then with probability at least $1-\delta$ we have that

$$ \epsilon(\hat{h}) \leq (min_{h \in H} \epsilon(h)) + 2  \sqrt{\frac{1}{m}log\frac{2k}{\delta}}$$


**Corollary** let any $\delta$, $\gamma$ 
$$ m \geq \frac{1}{2\gamma^2}log\frac{2k}{\delta}
      = O(\frac{1}{\gamma^2}log\frac{2k}{\delta}) $$
      
      
## The case of infinite H ##

**Intuition** :

H is parameterized by d real numbers, $m  > O(d)$, the number of training examples needed is at most linear linear in parameters of the model

$H$ **shatters** S if H can realize any labeling of S

$S = \{x^{i},.....,x^{d} \}$ for any set of labels, there exists some $ h \in H$ so that $h(x^{(i)}) = y^{i}$ for all $i=1,.....d$

**Vapnik-Chervonenkis dimension** VC(H):

the size of largest set that is shattered by H


**Theorem** Let H be given, and let d = VC(H), then wih probability at least $1 - \delta$, we have that for all $h \in H$,

$$ | \epsilon(h) -\epsilon(\hat{h}) | \leq O(\sqrt{\frac{d}{m}log\frac{m}{d} + \frac{1}{m}log\frac{1}{\delta}})  $$

Thus, with probability at least $1 - \delta$, we also have
$$ \epsilon(\hat{h}) \leq \epsilon(h) + O(\sqrt{\frac{d}{m}log\frac{m}{d} + \frac{1}{m}log\frac{1}{\delta}}) $$

### Conclusion ###

the number of training examples needed is usually roughly **linear** in the number of parameters of H
