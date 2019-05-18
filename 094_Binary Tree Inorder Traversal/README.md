[94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

Medium

Given a binary tree, return the *inorder* traversal of its nodes' values.

**Example:**

```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]
```

**Follow up:** Recursive solution is trivial, could you do it iteratively?

给你一个二叉树，求出此二叉树的中序遍历。

C++：

递归解法：

```c++
/**
Runtime: 0 ms, faster than 100.00% of C++ online submissions for Binary Tree Inorder Traversal.
Memory Usage: 9.6 MB, less than 11.74% of C++ online submissions for Binary Tree Inorder Traversal.

 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        dfs(root,res);
        return res;
    }
    void dfs(TreeNode* root,vector<int> &res)
    {
        if (!root) return ;
        if(root->left)
            dfs(root->left,res);
        res.push_back(root->val);
        if(root->right)
            dfs(root->right,res);
        return;
    }
};
```

栈模拟递归：

```c++
/**

Runtime: 0 ms, faster than 100.00% of C++ online submissions for Binary Tree Inorder Traversal.
Memory Usage: 9.3 MB, less than 37.07% of C++ online submissions for Binary Tree Inorder Traversal.


 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> s;
        while(root||!s.empty())
        {
            while(root)
            {
                s.push(root);
                root=root->left;
            }
            root = s.top();
            s.pop();
            res.push_back(root->val);
            root = root->right;
        }
        return res;
    }
};
```

Python3:

```python
'''
Runtime: 28 ms, faster than 99.82% of Python3 online submissions for Binary Tree Inorder Traversal.
Memory Usage: 13.2 MB, less than 24.95% of Python3 online submissions for Binary Tree Inorder Traversal.
'''

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        s = []
        while root is not None or len(s)>0:
            while root is not None:
                s.append(root)
                root = root.left
            root = s[-1]
            s.pop()
            res.append(root.val)
            root = root.right
        return res
```

