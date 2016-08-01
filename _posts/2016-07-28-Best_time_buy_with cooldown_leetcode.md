---
layout: post
title: Best Time to Buy with Cooldown
date: 2016-07-28
categories: blog
tags: [Leetcode, Array]
description: Leetcode Solution
---


[Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)


Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)


**Example**



prices = [1, 2, 3, 0, 2]

maxProfit = 3

transactions = [buy, sell, cooldown, buy, sell]


Solution:

Then the transaction sequences can end with any of these three states.

For each of them we make an array, buy[n], sell[n] and rest[n].

buy[i] means before day i what is the maxProfit for any sequence end with buy.

sell[i] means before day i what is the maxProfit for any sequence end with sell.

rest[i] means before day i what is the maxProfit for any sequence end with rest.



```
buy[i] = max(buy[i-1], reset[i-1]-price)

reset[i] = max(reset[i-1], sell[i-1], buy[i-1])

sell[i] = max(buy[i-1]+price, sell[i-1])
```


To make sure buy-reset-buy is not going to happen, observe that

reset[i] >= buy[i-1], so

reset[i] = max(reset[i-1], sell[i-1])

reset[i] <= sell[i] => reset[i] == sell[i-1]

so

```
buy[i] = max(buy[i-1], sell[i-2]-price)

sell[i] = max(sell[i-1], buy[i-1]+price)


```

The final solution is 


```Python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        if len(prices) < 2:
            return 0
        sell,buy = 0,-prices[0]
        pre_sell, pre_buy = 0,0
        for price in prices:
            pre_buy = buy
            buy = max(pre_sell - price, pre_buy)
            pre_sell = sell
            sell = max(pre_buy + price, pre_sell)
            
        return sell


```

