---
layout: post
title: The All-pairs short Paths Problem
date: 2016-08-09
categories: blog
tags: [Algorithms,Notes, Dynamics programming]
description: The All-pairs short Paths Problem
---

### Problem Definition

**Input:**

Directed graph G = (V , E ) with edge costs ce for each edge e ∈ E .

**Goal:**

* Compute the length of a shortest u → v path for all pairs ofvertices u, v ∈ V

* Correctly report that G contains a negative cycle.

**N** invocations of a single-source shortest-path subroutine are needed to solve the all-pairs shortest path problem?

**Running time (nonnegative edge costs):**

V· **Dijkstra** = O(V E logV)  

$O(n^3 \log{n}) \mbox{if  m }= O(n^2)$

$O(n^2 \log{n}) \mbox{if  m = O(n)}$

**Running time (general edge costs):**

V **Bellman-Ford** = $O(V^2 E)$

$O(n^3 ) \mbox{if  m }= O(n)$

$O(n^4) \mbox{if  m }= O(n^2)$

### Floyd-Warshall algorithm

**Time Complexity** $O(n^3)$

* At least as good as n Bellman-Fords, better in dense graphs.

* In graphs with nonnegative edge costs, competitive with n Dijkstra’s in dense graphs.


### Optimal sub-problem

**Optimal substructure lemma:**

Suppose G has no negative cost cycle. Let P be a shortest (cycle-free) i-j path with all internal nodes in V (k). Then:

**Case 1**:

If k not internal to P, then P is a shortest (cycle-free) i-j path with all internal vertices in V(k−1).

**Case 2**:

P1 = shortest (cycle-free) i-k path with all internal nodes inV(k−1) 

P2 = shortest (cycle-free) k-j path with all internal nodes inV(k−1

### Code Implementation

```Python
def floyd_warshall_2D(graph):
    n = len(graph.keys())
    A = [[0 for _ in range(n)] for _ in range(n)]
    for i in graph.keys():
        for j in graph.keys():
            if i == j:
                A[i][i] = 0
            if j in graph[i]:
                A[i][j] = graph[i][j]
            else:
                A[i][j] = float("inf")
            if i in graph[j]:
                A[j][i] = graph[j][i]
            else:
                A[j][i] = float("inf")
    for k in range(n):
        for i in range(n):
            for j in range(n):
                if A[i][j] > A[i][k] + A[k][j]:
                    A[i][j] = A[i][k] + A[j][j]
    return A

```

### Johnson’s algorithm Intuition

1 invocation of Bellman-Ford  O(V E logV) 

n n invocations of Dijkstra $O(V E)$

#### Reweighting Technique

**Lemma 1:**

When all s-t paths in G have the same number of edges, adding a constant M to every edge **does not change** the shorted s-t path

**Lemma 2**:

**Set up**:

G = (V , E ) is a directed graph with general edge lengths $c_e$. Fix a real number $p_v$ for each vertex v ∈ V

**Definition:**

For every edge e=(u,v) of G, $c_e′$ := $c_e$ + $p_u$ − $p_v$

If s-t path has originial length L: the new length is $L + p_s - p_t$

**Summary:**

Reweighting using vertex weights $p_v$ adds the same amount (namely, $p_s$ − $p_t$ ) to every s -t path

**Consequence**

1. Reweighting always leaves the shortest path unchanged.

2. After re-weighting, all edegs become positive!!


### Johnson’s algorithm Implementation

**Input:** Directed graph G = (V , E ), general edge lengths $c_e$

1. Form G′ by adding a new vertex s and a new edge (s,v) withlength 0 for each v ∈ G

2. Run Bellman-Ford on G′ with source vertex s. [If B-F detects a negative-cost cycle in G′ (which must lie in G), halt + report this.]

3. For each v∈G,define pv =length of a shortest s→v path in G′. For each edgee = (u,v)∈G, define $c_e′$ := $c_e$ + $p_u$ − $p_v$

4. For each vertex u of G: Run Dijkstra’s algorithm in G, with edge lengths {ce′}, with source vertex u, to compute the shortest-path distance d′(u,v) for each v ∈ G.

5. For each pair u, v ∈ G , return the shortest-path distance d(u,v):=d′(u,v)−pu +pvp

