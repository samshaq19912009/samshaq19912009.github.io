---
layout: post
title: Binary Tree Level Order Traversal
date: 2016-08-09
categories: blog
tags: [Leetcode, Tree]
description: Leetcode Solution
---

[Leetcode Link](https://leetcode.com/problems/binary-tree-level-order-traversal/)




### Solution 1 : Using Queue

```Python
class Solution(object):
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        q = [root]
        ans = []
        while q:
            ans.append([node.val for node in q])
            new_q = []
            for node in q:
                if node.left:
                    new_q.append(node.left)
                if node.right:
                    new_q.append(node.right)
            q = new_q
        return ans

```


### Solution 2: Depth First Search


```Python
class Solution(object):
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        ans = []
        self.bfs(root,0, ans)
        return ans
        
    def bfs(self, node, level, ans):
        if not node:
            return
        
        if len(ans) < level+1:
            ans.append([])
        ans[level].append(node.val)
        self.bfs(node.left, level+1, ans)
        self.bfs(node.right, level+1, ans)


```