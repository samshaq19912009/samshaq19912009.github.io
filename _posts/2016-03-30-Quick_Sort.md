---
layout: post
title: Quick Sort
date: 2016-03-31
categories: blog
tags: [Algorithm,Notes]
description: Quick Sort
---

# Quick Sort

* a "Greatest hit" algorithm
* Prevalent in practise
* Beautiful analysis
* $O(n log n)$ time "on average"

Key idea : partition array aroud a pivot element

Pick element of array
Rearrange array so that
left of pivot less than pivot
right of pivot greater than pivot

QuickSort(array A, length n)

* if n =1 return
* p = ChoosePivot(A,n)
* Partition A around p
* Recursively sort 1st part
* Recursively sort 2nd part

**The easy way out**

Using O(n) extra memory, easy to partition around pivot in O(n) time

**In-place Implementation**

Assume: pivot = 1st element of array

[if not, swap pivot with 1st element]

**Pseudocode for Partition**



	Partition(A,l,r):
		p := A[l]
		i := l+1
		for j = l+1 to r:
			if A[j] < p:
				swap A[j] and A[i]
				i := i+1
		swap A[l] and A[i-1]

**QuickSort Theorem** : for every input array of length n, the average running time(with Random pivots) is O(nlog(n))

		

