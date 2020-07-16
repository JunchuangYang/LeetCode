# [863. 二叉树中所有距离为 K 的结点](https://leetcode-cn.com/problems/all-nodes-distance-k-in-binary-tree/)

给定一个二叉树（具有根结点 root）， 一个目标结点 target ，和一个整数值 K 。

返回到目标结点 target 距离为 K 的所有结点的值的列表。 答案可以以任何顺序返回。

示例 1：

输入：root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, K = 2
输出：[7,4,1]
解释：
所求结点为与目标结点（值为 5）距离为 2 的结点，
值分别为 7，4，以及 1

提示：

1. 给定的树是非空的。

2. 树上的每个结点都具有唯一的值 0 <= node.val <= 500 。
3. 目标结点 target 是树上的结点。
4. 0 <= K <= 1000.

**首先为每个节点加上父节点的指针，然后进行广搜。**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def distanceK(self, root: TreeNode, target: TreeNode, K: int) -> List[int]:
        self.bfs(root,None)
        res = []
        q = deque()
        q.append([target,0])
        visited = [target]
        while q:
            node,num = q.popleft()
            if num == K:
                res.append(node.val)
            for item in [node.left,node.right,node.par]:
                if item and item not in visited:
                    visited.append(item)
                    q.append([item,num+1])
        return res
    def bfs(self,root,parent):
        if not root:
            return
        # 为每一个结点增加指向父节点的直针
        root.par = parent
        self.bfs(root.left,root)
        self.bfs(root.right,root) 
   
```

