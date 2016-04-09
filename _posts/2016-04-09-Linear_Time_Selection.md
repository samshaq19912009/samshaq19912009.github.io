---
layout: post
title: Linear Time Selection
date: 2016-04-09
categories: blog
tags: [Algorithm,Notes]
description: Linear Time Selection
---



#### Linear Expectation
Let $X_1,....,X_n $ be random variables defined on $\Omega$, then :
$$ E[\sum_{j=1}^nX_j] = \sum_{j=1}^n E[X_j]
$$

**Prove**
$$ \sum_{j=1}^nE[X_j] = \sum_{j=1}^n\sum_{i \in \Omega} X_j(i)p(i) \\
 = \sum_{i \in \Omega}\sum_{j=1}^n X_j(i)p(i) \\ = \sum_{i \in \Omega}p(i) \sum_{j=1}^n X_j(i) \\ = E[\sum_{j=1}^nX_j]
$$

### The problem
Input: array A with n distinct numbers and a number 

Output :ith order statistic

#### Merge Sort Algorithm

Apply Merge Sort. return ith element of sorted array. ($O(n\log n$)

**Fact:**can't sort any faster

#### Randomized Selection

```
Rselect(array A, length n,order i):
if n = 1 return A[1]
Choose pivot p from A uniformly at random
Partition A around p let j = new index of p
if j = i return p
if j > i return Rselect(1st part of A, j-1, i)
if j < i return Rselect(2nd part of A, n-j, i-j)
```

**Worst Case performance** $O(n^2)$

**Key:** find pivot giving "balanced" split

$$T(n) \leq T(n/2) + O(n)$$

Best pivot: the median. $T(n) = O(n)$

**Rselect Theorem:** for every input array of length n, the average running time of Rselect is $O(n)$

#### The DSelect Algorithm

```
Dselect(array A, length n,order i):
Break A into groups of 5, sort each group
C = the n/5 "middle element"
p = DSelect(C,n/5,n/10)
Partition A around p let j = new index of p
if j = i return p
if j > i return Dselect(1st part of A, j-1, i)
if j < i return Dselect(2nd part of A, n-j, i-j)
```

**Run time analysis**

1. T(1) = 1
2. $T(n) \leq c*n + T(n/5) + T(7n/10)$

**Key Lemma:** 2nd recursive call guaranteed to be an array of size <= 7n/10

Strategy:**hope and check**(can't use Master Method)

### A Sorting Lower Bound

**Theorem:**every "comparison-based" sorting algorithm has worst-case running time $O(n\log n)$

**Proof:**
Consider input arrays containing {1,2,3,...n} in some order, suppose algorithm always makes k comparison, then
$$2^k \geq n! \geq (\frac{n}{2})^{n/2}$$


