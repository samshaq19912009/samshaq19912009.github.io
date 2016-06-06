---
layout: post
title: Dijkstra Algorithm 
date: 2016-06-06
categories: blog
tags: [Algorithms,Notes]
description: Dijkstra Algorithm
---




# Dijkstra Algorithm

Input: Given a graph and a source, compute the smallest distance from the source to all the other node


Worst Running Time :  $O(E + V \log V)$

```Python

##First read the data

f = open("./dijkstraData.txt", "r")
graph = {}

for l in f.readlines():
    #print int(l.split()[0]), map(int,l.split()[1].split(','))
    
    data = l.split()
    node1 = int(data[0])
    for input_num in data[1:]:
        node2, weight = map(int,input_num.split(','))
        temp = {}
        temp[node2] = weight
        if node1 in graph.keys():
            graph[node1].update(temp)
        else:
            graph[node1] = temp


```
With heap implementaion




```Python

from heapq import heappop, heappush


def dij_short_path(graph, s):
    infi = 10000
    weights = [infi] * (len(graph)+1)
    weights[s] = 0
    n  = len(graph)
    Q = [(0,s)]
    visited = [False] * (len(graph)+1)
    
    while len(Q) > 0:
        (cost, u) = heappop(Q)
        if visited[u]:
            continue
        else:
            visited[u] = True
        
            for v in graph[u]:
                weights[v] = min(weights[v], weights[u]+ graph[u][v])
                heappush(Q, (weights[v], v))

        
    return weights
```    