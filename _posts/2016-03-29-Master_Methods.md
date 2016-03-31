---
layout: post
title: Master Methods
date: 2016-03-29
categories: blog
tags: [Algorithm,Notes]
description: Master Methods
---

# Master Method

Assumption : all subproblems have equal size.

1. Base Case $T(n)$ <= a constant for all sufficiently small n
2. For all larger n :
$$T(n) \leq aT(n/b) + 0(n^d) $$
where
	
	a = number of recursive calls $\geq 1$
	
	b = input size shrinkage
	
	d = exponent in running time
	
\begin{equation}
    T(n)=
   \begin{cases}
   O(n^d log n) &\mbox{if $a=b^d$}\\ \newline
   O(n^d) &\mbox{if $a < b^d$} \newline
   O(n^{log_{b}a}) &\mbox{if $a > b^d$}
   \end{cases}
  \end{equation}
  
  **Example 1: Merge Sort**
  
  a = 2 b= 2 d= 1 -> $b^d = a$ -> case 1
  $$T(n) = O(nlogn)$$
  
  
  **Example 2: Binary Search**
  
  
  a = 1 b= 2 d= 0 -> $b^d = a$ -> case 1
  $$T(n) = O(logn)$$
  
  **Example 3: Integer Multiplication**
  
  $$T(n) =  4T(n/2) + 0(n) $$
  
  a = 4 b= 2 d= 1 -> $b^d < a$ -> case 3
  $$T(n) = O(n^2)$$
  
  
   **Example 3: Gauss Multiplication**
  
  $$T(n) =  3T(n/2) + 0(n) $$
  
  a = 3 b= 2 d= 1 -> $b^d < a$ -> case 3
  $$T(n) = O(n^{log_2{3}})$$
  
   
   **Example 4: Strassen's Matrix Multiplication**
  
  $$T(n) =  7T(n/2) + 0(n^2) $$
  
  a = 7 b= 2 d= 2 -> $b^d < a$ -> case 3
  $$T(n) = O(n^{log_2{7}})$$
  
### Proof 
At each level $j=0,1,2,...log_b{n}$ there are $a^j$ subproblems, each of size of $n/b^j$

$$ \mbox{Total} \leq cn^d \cdot \sum_{j=0}^{log_b n} (\frac{a}{b^d})^j   $$

**Upper bound** for each level j : $cn^d \times (\frac{a}{b^d})^j  $

a = rate of subproblem proliferation (RSP)

$b^d$ = rate of work shrinkage (RWS)

1. RSP = RWS $\rightarrow$ Same amout of work each level(like merge sort) expect $0(n^d log n)$
2. RSP < RWS $\rightarrow$ less amout of work each level(most work at root) migt expect $0(n^d log n)$
3. RSP > RWS $\rightarrow$ more amout of work each level(most work at leaves) migt expect $0(\mbox{# of leaves})$
  
  
	
