## [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

Medium

Given inorder and postorder traversal of a tree, construct the binary tree.

**Note:**
You may assume that duplicates do not exist in the tree.

For example, given

```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```

题意：给出中序和后序序列，求二叉树

C++版：

```c++
/**
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
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        int instart = 0,inend = inorder.size()-1,poststart = 0,postend = postorder.size()-1;
        return dfs(inorder,postorder,instart,inend,poststart,postend);
    }
    TreeNode* dfs(vector<int>& inorder, vector<int>& postorder,int instart,int inend,int poststart,int postend)
    {
        if (instart >inend || poststart >postend)
            return NULL;
        TreeNode *node = new TreeNode(postorder[postend]);
        int index=0;
        for(int i=instart;i<=inend;i++)
        {
            if(inorder[i] == postorder[postend])
            {
                index=i;//根节点
                break;
            }
        }
        //instart,index-1：以postorder[postend]为根的左子树的中序遍历的起始和结束位置。
        //poststart,poststart+index-instart-1：以postorder[postend]为根的左子树的后序遍历的起始和结束位置。
        node->left = dfs(inorder,postorder,instart,index-1,poststart,poststart+index-instart-1);
        node->right= dfs(inorder,postorder,index+1,inend,poststart+(index-instart),postend-1);
        return node;

    }
};
```

Python3版：

```python
/*
Runtime: 236 ms, faster than 17.67% of Python3 online submissions for Construct Binary Tree from Inorder and Postorder Traversal.
Memory Usage: 87.8 MB, less than 5.26% of Python3 online submissions for Construct Binary Tree from Inorder and Postorder Traversal.
*/

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        if len(postorder) == 0:
            return None
        node = TreeNode(postorder[-1])
        index = inorder.index(postorder[-1])
        node.left = self.buildTree(inorder[0:index],postorder[0:index])
        node.right = self.buildTree(inorder[index+1:],postorder[index:len(postorder)-1])
        return node
    
        
```

