# [98. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。
示例 1:

输入:
​    2
   / \
  1   3
输出: true
示例 2:

输入:
​    5
   / \
  1   4
​     / \
​    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
​     根节点的值为 5 ，但是其右子节点值为 4 。

**我的思路是递归求左右子树的最大和最小值，保存起来，与根节点比较。我想了很长时间，也写的比较麻烦**

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        self.flag = 1
        self.dfs(root,0)
        if self.flag:
            return True
        return False

    def  dfs(self,root,v):
        if not root or self.flag==0:
            return None,None
        left,leftmin = self.dfs(root.left,1)
        right,rightmin = self.dfs(root.right,2)
        if left is None and right is None:
            return root.val,root.val
        elif left is not None and right is None :
            if left>=root.val or leftmin>=root.val:
                self.flag=0
                return None,None
            return max(left,root.val),min(left,root.val)
        elif right is not None and left is None:
            if right<=root.val or rightmin<=root.val:
                self.flag=0
                return None,None
            return max(right,root.val),min(right,root.val)
        else:
            if left>=root.val or leftmin>=root.val or rightmin<=root.val or right<=root.val:
                self.flag=0
                return None,None
            return max(max(right,root.val),left),min(min(right,root.val),left)
```

**[官方题解](<https://leetcode-cn.com/problems/validate-binary-search-tree/solution/yan-zheng-er-cha-sou-suo-shu-by-leetcode-solution/>)**

**思路一：递归，也是保留最大值最小值，比我的方法巧妙的太多了。**

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        return self.dfs(root)
    def dfs(self,root,lower=float('-inf'),upper=float('inf')):
        if not root:
            return True
        if root.val<=lower or root.val>=upper:
            return False
        # 先右递归（先左递归也可以过）
        if not self.dfs(root.right,root.val,upper):
            return False
        if not self.dfs(root.left,lower,root.val):
            return False
        return True
```

**思路二：中序遍历。二叉搜索树的中序遍历是一个递增数列，所以可以判断当前节点的值是否比前一个节点大来验证是否是二叉搜索树。**

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        self.pre = []
        if root is not None and not root.left and not root.right:
            return True
        return self.dfs(root)
    def dfs(self,root):
        if not root:
            return True
        if not self.dfs(root.left):
            return False
        if len(self.pre)>0:
            if self.pre[-1].val>=root.val:
                return False
        self.pre.append(root)
        if not self.dfs(root.right):
                return False
        return True
```

**非递归版本：**

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        inorder = float('-inf')
        self.pre = []
        while self.pre or root:
            # 左子树
            while root:
                self.pre.append(root)
                root = root.left
            root = self.pre.pop()
            if root.val<=inorder:
                return False
            inorder = root.val
            # 右子树
            root = root.right
        return True
```

