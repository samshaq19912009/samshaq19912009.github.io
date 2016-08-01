---
layout: post
title: Leetcode Find Kth Largest
date: 2016-07-28
categories: blog
tags: [Leetcode, sort]
description: Leetcode Solution
---

[Link](https://leetcode.com/problems/kth-largest-element-in-an-array/)


```Python
class Solution(object):
    def findKthLargest(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        pivot = nums[0]
        tail = 0
        for i in range(1, len(nums)):
            if nums[i] > pivot:
                tail += 1
                nums[i], nums[tail] = nums[tail], nums[i]
        nums[0], nums[tail] = nums[tail], nums[0]
        
        if tail+ 1 == k:
            return pivot
        elif tail+1 < k:
            return self.findKthLargest(nums[tail+1:], k-tail-1)
        else:
            return self.findKthLargest(nums[:tail], k)
            


```

### Randomnized Quick Select

```Python

import random
class Solution(object):
    def findKthLargest(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        pivot = random.choice(nums)
        num1, num2 = [], []
        for num in nums:
            if num > pivot:
                num1.append(num)
            elif num < pivot:
                num2.append(num)
        if k <= len(num1):
            return self.findKthLargest(num1, k)
        elif k > len(nums) - len(num2):
            return self.findKthLargest(num2, k- (len(nums) - len(num2)))
        
        return pivot


```