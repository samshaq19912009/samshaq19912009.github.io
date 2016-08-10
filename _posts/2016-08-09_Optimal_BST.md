---
layout: post
title: Optimal Binary Search Tree
date: 2016-08-09
categories: blog
tags: [Algorithms,Notes, Dynamics programming]
description: Optimal Binary Search Tree
---

### Problem Descirption

**Recall**:

For a given set of keys, there are lots of valid search Tree

![](https://upload.wikimedia.org/wikipedia/commons/d/da/Binary_search_tree.svg)

**Question**

What is the "best" search tree

**A good answer**:

A balanced search tree

**Input**:

Frequencies $p1, p2, p_n$ for items $1,2,....n$

**Goal**:

Compute a valid search tree that minimized the weighted average search time:

$C(T) = \sum_{items i}p_i * [\mbox{search time in T}]$

#### Comparison with Huffman Codes

**Similarities:**:

* Output = a binary tree

* Goal is (essentially) to minimize average depth with respect togiven probabilities

**Differences**:

* With Huffman codes, constraint was prefix-freeness

* Constraint = search tree property



### Greedy Doesn't Work

**Issue**

With the top-down approach, the choice of root has hard-to-predict repercussions further down the tree.

### Optimal Substructure

Suppose an optimal BST for keys {1, 2, . . . , n} has root r, left subtree T1, right subtree T2.

T1 is optimal for the keys {1,2,...,r −1} and T2 for the keys {r +1,r +2,...,n}


#### Proof of  Optimal Substructure

$C(T) = C(T_1) + C(T_2)$


### The Recurrence

**Notation:** For 1 ≤ i ≤ j ≤ n, let $C_{ij}$ = weighted search cost of an optimal BST for the items {i, i + 1, . . . , j − 1, j} [with probabilities pi,pi+1,...,pj]


**Recurrence:** 
$C_{i j} = min_{r=i,..j}\{ \sum_{k=i}^j p_k + C_{i,r-1} + C_{r+1, j} \}$


### Running Time

* $O(n^2)$ subproblems

* $O(j-i)$ time to compute subproblem

* $O(n^3)$ time overall

### Python implementation

```
def optimal_BST(key, freq):
    n = len(key)
    cost = [[0 for i in range(n)] for i in range(n)]
    for i in range(n):
        cost[i][i] = freq[i]
    for s in range(1, n):#s means i-j
        for i in range(n-s):
            j = i+s
            
            cost[i][j] = 10000
            for r in range(i,j+1):
                left = cost[i][r-1] if r > i else 0
                right = cost[r+1][j] if r < j else 0
                temp = left + right + sum_freq(freq,i,j)
                if temp < cost[i][j]:
                    cost[i][j] = temp
    return cost[0][n-1]
    
    
def sum_freq(freq, i, j):
    total = sum(freq[i:j+1])
    
    return total


```



