# [101. 对称二叉树](https://leetcode-cn.com/problems/symmetric-tree/)

给定一个二叉树，检查它是否是镜像对称的。

```python
例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    1
   / \
  2   2
 / \ / \
3  4 4  3
 

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    1
   / \
  2   2
   \   \
   3    3
 

进阶：

你可以运用递归和迭代两种方法解决这个问题吗？
```

**递归：一个左递归，一个右递归。**

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if self.dfs_left(root) == self.dfs_right(root):
            return True
        return False
    def dfs_left(self,root):
        if not root:
            return ['']
        return [root.val] + self.dfs_left(root.left) + self.dfs_left(root.right)
    def dfs_right(self,root):
        if not root:
            return ['']
        return [root.val] + self.dfs_right(root.right) + self.dfs_right(root.left)
```

**迭代：层次遍历**

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root:
            return True
        res = []
        res.append([root.left,root.right])
        while len(res)>0:
            t = res.pop(0)
            l,r = t[0],t[1]
            if l is not None and r is not None:
                if l.val != r.val:
                    return False
                res.append([l.left,r.right])
                res.append([l.right,r.left])
            elif  l is not None or r is not None:
                return False
        return True 
```

