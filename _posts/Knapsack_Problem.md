---
layout: post
title: Knapsack Problem
date: 2016-06-06
categories: blog
tags: [Algorithms,Notes]
description: Dynamic Programming, Knapsack
---








## Knapsack Problem





### 2D Solution


```Python
def knapsack(value, weight, wt, length):
    k = [ [0 for i in range(wt+1)] for j in range(length+1)]
    for w in range(wt+1):
        for l in range(length+1):
            if w == 0 or l == 0:
                k[l][w] = 0
            elif weight[l-1] > w:
                k[l][w] = k[l-1][w]
            else:
                k[l][w] = max(k[l-1][w], value[l-1] + k[l-1][w-weight[l-1]])
    return k[length][wt]


```


### 1D Solution

```Python
def knapsack_1D(value, weight, wt, length):
    k = [0 for i in range(wt+1)]
    for i in range(length):
        for j in range(wt,0,-1):
            if weight[i] <= j:
                k[j] = max(k[j], k[j-weight[i]]+value[i])
            else:
                k[j] = k[j]
            
    return k[-1]





```