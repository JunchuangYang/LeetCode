# [124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

给定一个**非空**二叉树，返回其最大路径和。

本题中，路径被定义为一条从树中任意节点出发，达到任意节点的序列。该路径**至少包含一个**节点，且不一定经过根节点。

```
示例 1:

输入: [1,2,3]

       1
      / \
     2   3

输出: 6
示例 2:

输入: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

输出: 42
```

**递归,记录最大值。有坑**

```
例如：
   -10
   / \
  9  20
    /  \
   15   7
 取的是：20,15,7
   10
   / \
  9  20
    /  \
   15   7
 取的是：10，20,7
```

**所以最大值只能记录，返回的时候一定要包含根节点。**

我写的还是麻烦。官解里总有简单而清除的递归。

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def maxPathSum(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        self.num = float('-inf')
        self.num = max(self.dfs(root),self.num)
        return self.num
    def dfs(self,root):
        if not root:
            return float('-inf')
        left = self.dfs(root.left)
        right = self.dfs(root.right)
        self.num = max(left,right,root.val,self.num)
        if left == float('-inf') and right == float('-inf'):
            return root.val
        elif left == float('-inf') :
            return max(root.val+right,root.val)
        elif right == float('-inf'):
            return max(root.val+left,root.val)
        else:
            self.num = max(root.val+right+left,root.val+left,root.val+right,root.val,self.num)
            return max(root.val+left,root.val+right,root.val)
```

**官解**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def maxPathSum(self, root: TreeNode) -> int:
        def max_gain(node):
            nonlocal max_sum
            if not node:
                return 0
            left_gain = max(max_gain(node.left), 0)
            right_gain = max(max_gain(node.right), 0)
            price_newpath = node.val + left_gain + right_gain # 记录最大最小值
            max_sum = max(max_sum, price_newpath) # 记录最大最小值
            # 根节点必须包含，后选择加左右子节点中的一个
            return node.val + max(left_gain, right_gain) 
        max_sum = float('-inf')
        max_gain(root)
        return max_sum
```

