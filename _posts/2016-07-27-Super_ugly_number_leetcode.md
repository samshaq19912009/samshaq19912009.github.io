---
layout: post
title: [Leetcode] Super Ugly Number
date: 2016-07-26
categories: blog
tags: [Leetcode, heap]
description: Leetcode Solution
---

[Link](https://leetcode.com/problems/super-ugly-number/)


Write a program to find the nth super ugly number.

Super ugly numbers are positive numbers whose all prime factors are in the given prime list primes of size k. For example, [1, 2, 4, 7, 8, 13, 14, 16, 19, 26, 28, 32] is the sequence of the first 12 super ugly numbers given primes = [2, 7, 13, 19] of size 4.

Note:
(1) 1 is a super ugly number for any given primes.
(2) The given numbers in primes are in ascending order.
(3) 0 < k ≤ 100, 0 < n ≤ 106, 0 < primes[i] < 1000.

题目解析：

可以使用[Ugly Number II](https://leetcode.com/problems/ugly-number-ii/)的解法，开辟一个index的序列记录primen number的下标。每次更新选择其中一个最小的，但这样的解法太慢。比较好的是利用heapq和yield 生成器。具体代码如下

```Python
class Solution(object):
    def nthSuperUglyNumber(self, n, primes):
        """
        :type n: int
        :type primes: List[int]
        :rtype: int
        """
        ans = [1]
        def gen(prime):
            for x in ans:
                yield x * prime
        merged = heapq.merge(*map(gen, primes))
        while len(ans) < n:
            ugly = next(merged)
            if ugly != ans[-1]:
                ans.append(ugly)
        return ans[-1]

```