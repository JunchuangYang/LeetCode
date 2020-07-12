#  [99. 恢复二叉搜索树](https://leetcode-cn.com/problems/recover-binary-search-tree/)

二叉搜索树中的两个节点被错误地交换。

请在不改变其结构的情况下，恢复这棵树。

```
示例 1:

输入: [1,3,null,null,2]

   1
  /
 3
  \
   2

输出: [3,1,null,null,2]

   3
  /
 1
  \
   2
示例 2:

输入: [3,1,4,null,null,2]

  3
 / \
1   4
   /
  2

输出: [2,1,4,null,null,3]

  2
 / \
1   4
   /
  3
进阶:

使用 O(n) 空间复杂度的解法很容易实现。
你能想出一个只使用常数空间的解决方案吗？
```

[题解](<https://leetcode-cn.com/problems/recover-binary-search-tree/solution/zhong-xu-bian-li-by-powcai/>)

**中序遍历：递归**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def recoverTree(self, root: TreeNode) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        self.firstNode,self.secondNode = None,None
        self.pre = TreeNode(float('-inf'))
        self.inorder(root)
        self.firstNode.val,self.secondNode.val = self.secondNode.val,self.firstNode.val
    def inorder(self,node):
        if not node:
            return
        self.inorder(node.left)
        # 第一次出现前一个节点值大于后一个节点值时，记录前一个节点
        if not self.firstNode and self.pre.val > node.val:
            self.firstNode = self.pre
        # 第二次出现前一个节点值大于后一个节点值时，记录后一个节点
        if self.firstNode and self.pre.val>node.val:
            self.secondNode = node
        self.pre = node
        self.inorder(node.right)

        return 
```

