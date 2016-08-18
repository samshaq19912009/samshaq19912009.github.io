## 222. Count Complete Tree Nodes

[Leetcode Link](https://leetcode.com/problems/count-complete-tree-nodes/)

Given a complete binary tree, count the number of nodes.

Brutal Force : Time $O(n)$ : **TLE error**

Get the node depth first: Time $O(\log^2 n)$

```Python
class Solution(object):
    def countNodes(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        l = self.getDepth(root, True)
        r = self.getDepth(root, False)
        if l == r:
            return int(pow(2,l)) - 1
        else:
            return 1 + self.countNodes(root.left) + self.countNodes(root.right)
        
    def getDepth(self, root, isleft):
        if not root:
            return 0
        level = 0
        while root:
            
            if isleft:
                root = root.left
            else:
                root = root.right
            level += 1
        return level

```

## 235. Lowest Common Ancestor of a Binary Search Tree

Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

[Leetcode Link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

```Python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if not root:
            return root
        if max(p.val,q.val) < root.val:
            return self.lowestCommonAncestor(root.left, p, q)
        elif min(p.val, q.val) > root.val:
            return self.lowestCommonAncestor(root.right, p, q)
        return root
```

## 236. Lowest Common Ancestor of a Binary Tree

[Leetcode Link](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if not root or root == p or root == q:
            return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if left and right:
            return root
        if left:
            return left
        if right:
            return right

```



## 257. Binary Tree Paths

Given a binary tree, return all root-to-leaf paths.

[Leetcode Link](https://leetcode.com/problems/binary-tree-paths/)

```Python
class Solution:
    # @param {TreeNode} root
    # @return {string[]}
    def binaryTreePaths(self, root):
        if not root:
            return []
        tmp = []
        ans = []
        self.dfs(root, tmp,ans)
        return ans
    def dfs(self, node, tmp, ans):
        if not node:
            return
        tmp.append(str(node.val))
        if not node.left and not node.right:
            ans.append("->".join(tmp))
        if node.left:
            self.dfs(node.left, tmp, ans)
        if node.right:
            self.dfs(node.right, tmp, ans)
        tmp.pop()

```




## 337. House Robber III

[Leetcode Link](https://leetcode.com/problems/house-robber-iii/)

**Key Idea:**

For a single node, mark the both the result of robbing this node or not robbing this node.


You can not steal root and (root.left root.right) at the same time

```Python
class Solution(object):
    def rob(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        result = self.dfs(root)
        return result[0]
        
    def dfs(self, root):
        if not root:
            return 0, 0
        rob_l, no_rob_l = self.dfs(root.left)
        rob_r, no_rob_r = self.dfs(root.right)
        root_result = max( root.val+no_rob_l+no_rob_r, rob_l+rob_r)
        return root_result, rob_l+rob_r
```
