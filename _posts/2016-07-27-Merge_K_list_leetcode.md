---
layout: post
title: Leetcode Merge K List
date: 2016-07-28
categories: blog
tags: [Leetcode, sort]
description: Leetcode Solution
---

[Link](https://leetcode.com/problems/merge-k-sorted-lists/)






```Python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        def merge(l1, l2):
            dummy = ListNode(-1)
            cur = dummy
            while l1 and l2:
                if l1.val < l2.val:
                    cur.next = l1
                    l1 = l1.next
                else:
                    cur.next = l2
                    l2 = l2.next
                cur = cur.next
            cur.next = l1 or l2
            return dummy.next
        def helper(lists,begin, end):
            if begin > end:
                return 
            if begin == end:
                return lists[begin]
            mid = (begin+end)/2
            return merge(helper(lists,begin, mid) , helper(lists, mid+1, end))
        return helper(lists, 0, len(lists)-1)


```