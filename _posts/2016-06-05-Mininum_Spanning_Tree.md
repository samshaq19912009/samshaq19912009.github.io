---
layout: post
title: MST---Prim Algorithm
date: 2016-06-05
categories: blog
tags: [Algorithms,Notes, Greedy]
description: MST Prim
---

## Minium Spanning Tree

A minimum spanning tree is a spanning tree of a connected, undirected graph. It connects all the vertices together with the minimal total weighting for its edges. A single graph can have many different spanning trees. **No cycle allowed**







## Prim Algorithm

Inpute : G(V,E)

Running Time: O(E $\log V$)


### PesudoCode

1. Initialize X = {s}, s is a arbitrary source

2. T = empty set

3. While X is not all the vertice:
	
	Let e = (u,v) be the cheapest edge with u $\in$ X and v $\notin X$
	
	add e to T
	
	add v into X
	
4. Return T 



### Python Implementation



```Python
from heapq import heappop, heappush

def prim_st(G,s):# G : the graph s: the starting point
    V, T = [], {} #V: vertices in MST, T: MST
    #Priority Queue (weight, edge1, edge2)
    total = 0
    Q = [(0, None, s)]
    while Q:
        cost, p, u = heappop(Q) #choose edge w/ smallest weight
        if u in V: continue #skip any vertices already in MST
        V.append(u)
        #build MST structure
        if p is None:
            pass
        elif p in T:
            T[p].append(u)
            total += cost
        else:
            T[p] = [u]
            total += cost
        
        
        for v, w in G[u].items(): #add edges to fringe
            heappush(Q, (w, u, v))
    
    return T, total

```

## Kruskal’s MST Algorithm


Inpute : G(V,E)

Running Time: O(E $\log V$)


### PesudoCode

1. Initialize the tree

2. Sort the edge

3. Try the smallest edge each time, add the vertice into the tree if there is not crossing edge in T


	




### Python Implementation

```
from u import UnionFind

def MinimumSpanningTree(G):

    """
    Returning the minimum spanning tree
    """

    for u in G:
        for v in G[u]:
            if G[u][v] != G[v][u]:
                raise ValueError("MST: not symmetric")

    subtrees = UnionFind()
    tree = []
    for W,u,v in sorted((G[u][v],u,v) for u in G for v in G[u]):
        if subtrees[u] != subtress[v]:
            tree.append((u,v))
            subtress.union(u,v)

    return tree




```	




