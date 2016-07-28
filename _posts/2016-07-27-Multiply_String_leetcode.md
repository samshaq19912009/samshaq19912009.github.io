---
layout: post
title: [Leetcode] Multiply String
date: 2016-07-27
categories: blog
tags: [Leetcode, Math]
description: Leetcode Solution
---

[Link](https://leetcode.com/problems/multiply-strings/)

Given two numbers represented as strings, return multiplication of the numbers as a string.

Note:
The numbers can be arbitrarily large and are non-negative.
Converting the input string to integer is NOT allowed.
You should NOT use internal library such as BigInteger.

题目解析：

先将字符串翻转，从末尾开始计数。计算临时结果，先不要进位。

然后开始计算进位，每次保留carry向前一位。在ans的开头插入每一步的结果。

```Python
class Solution(object):
    def multiply(self, num1, num2):
        """
        :type num1: str
        :type num2: str
        :rtype: str
        """
        num1 = num1[::-1]
        num2 = num2[::-1]
        ans = [0] * (len(num1)+len(num2))
        for i in range(len(num1)):
            for j in range(len(num2)):
                ans[i+j] += int(num1[i]) * int(num2[j])
        
        result = []
        for i in range(len(ans)):
            digit = ans[i] % 10
            carry = ans[i] / 10
            if i < len(ans) - 1:
                ans[i+1] += carry
            result.insert(0, str(digit))
        while result[0] == '0' and len(result) > 1:
            del result[0]
        return "".join(result)
            


```