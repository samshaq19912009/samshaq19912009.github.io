---
layout: post
title: The Bellman-Ford Algorithm
date: 2016-08-09
categories: blog
tags: [Algorithms,Notes, Dynamics programming]
description: The Bellman-Ford Algorithm
---


**Input:**

Directed graph G = (V,E), edge lengths $c_e$ for each e ∈ E, source vertex s ∈ V . [Can assume no parallel edges.]


**Goal:** 

For every destination v ∈ V , compute the length (sum of edge costs) of a shortest s-v path.

### On Dijkstra’s Algorithm

**Good News**

O(E log V) running time using heaps

**Bad News**

* Not always correct with negative edge lengths

* Not very distributed


### Optimal Substructure

Suppose the input graph G has no negative cycles, **For every v, there is a shortest s-v path with ≤ n − 1 edges**


**Key idea:**

Artificially restrict the **number of edges** in a path. Subproblem size ⇐⇒ Number of permitted edges

### The Recurrence

**Notation**

$L_{i,v}$ minimum length of a s-v path, with no bigger than i edges

**Recurrence**

$L_{i,v}$ = min ( $L_{i-1,v}$, $min_{u,v \in E} L_{i-1,w} + C_{wv}  $ )

### Running Time

$O(n^2)$ Space Complexity

Outer loop: V loop

Inner Choice: $\sum_{v \in V} \mbox(indeg) V$

Time $O(E * V)$

### Python Implentation


```Python
def bellman_ford(graph,	node):
    n =	len(graph.keys())
    distance = [float("inf") for _ in range(n)]
    predecessor	[ None for _ in	range(n)]
    distance[node] = 0
    #relax
    for	i in range(1,n-1):
	for u in graph.keys():
            for	v,w in graph[v]:
		if distance[u]+w<distance[v]:
                    distance[v] = distance[u]+w
                    predecessor[v] = u
    #check if there is any negative circle
    for	u in graph.keys():
	for v, w in graph[u]:
            if distance[v] > distance[u] + w:
		print "Found a negative circle!"
		return
    return distance, predecessor


```

