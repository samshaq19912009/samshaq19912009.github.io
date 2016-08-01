---
layout: post
title: Leetcode Spiral Matrix
date: 2016-07-28
categories: blog
tags: [Leetcode, Array]
description: Leetcode Solution
---

[Link](https://leetcode.com/problems/spiral-matrix/)

```Python
class Solution(object):
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """
        if not matrix:
            return []
        m, n = len(matrix), len(matrix[0])
        ans = []
        x_start, x_end = 0, n-1
        y_start, y_end = 0, m-1
        while True:
            ##from left to right
            for i in range(x_start, x_end+1):
                ans.append(matrix[y_start][i])
            ##from top to bottom
            y_start += 1
            if y_start > y_end:
                break
            
            for j in range(y_start, y_end+1):
                ans.append(matrix[j][x_end])
            ##from right t0 left
            x_end -= 1
            if x_end < x_start:
                break
            for i in range(x_end, x_start-1, -1):
                ans.append(matrix[y_end][i])
            ##from bottom to top
            y_end -=1
            if y_end < y_start:
                break
            for j in range(y_end, y_start-1, -1):
                ans.append(matrix[j][x_start])
            x_start += 1
            if x_start > x_end:
                break
        return ans
                


```