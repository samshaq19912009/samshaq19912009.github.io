---
layout: post
title: NP-Completeness
date: 2016-08-10
categories: blog
tags: [Algorithms,Notes]
description: NP-Completeness
---


## NP-Completeness


### P: Polynomial-Time Solvable Problems

**A problem is polynomial-time solvable if there is an algorithm that correctly solves it in $O(n^k)$ time, for some constant k.**

### The Class P

**P = the set of poly-time solvable problems**

### Reductions:

#### Conjecture: There is no polynomial-time algorithm that solves the TSP


**Problem Π1 reduces to problem Π2 if**

**Given a polynomial-time subroutine for Π2, can use it to solve Π1 in polynomial time.**

### Completeness

Let C = a set of problems.

The problem Π is C-complete if:

(1) Π ∈ C and (2) everything in C reduces to Π.

**Π is the hardest problem in all of C**

**Refined idea: TSP as hard as all brute-force-solvable problems.**

### The class NP:

Definition: A problem is in NP if:
(1) Solutions always have **length polynomial** in the input size 

(2) Purported solutions can be **verified in polynomial time.**

Note: Every problem in NP can be solved by brute-force search in exponential time.


### NP-Completeness User’s Guide

(1) Find a known NP-complete problem Π′ (see e.g. Garey + Johnson, Computers + Intractability)

(2) Prove that Π′ reduces to Π⇒ implies that Π at least as hard as Π′⇒ Π is NP-complete as well (assuming Π is an NP problem)

## The P vs. NP Question


### NP “Nondeterministic polynomial”