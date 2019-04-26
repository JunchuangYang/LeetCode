## [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

Medium

Given preorder and inorder traversal of a tree, construct the binary tree.

**Note:**
You may assume that duplicates do not exist in the tree.

For example, given

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```



**个人感觉这类二叉树的问题面试或笔试会有很大概率会被考到。**



题意：给出二叉树的前序和中序序列求二叉树。

对于下面的二叉树：

​                       7

​                    /    \

​                10        2

​              /     \      /

​           4        3     8

​                     \     /

​                     1    11

preorder的遍历结果：7  10  4  3  1  2  8  11

inorder的便利结果：  4  10  3  1  7  11 8  2

要解决这道题目，就要分别利用preorder和inorder遍历的两个性质。preoder的第一个元素确定为根节点，然后再在inorder里，它左侧的就是左子树的全部节点，右侧的就是右子树的全部节点。然后再对左子树这些节点，递归调用上面的方法。

比如，上面的例子，7为preorder的第一个元素，所以是整个树的根。那么在inorder中，7往左的4-1是左子树，往右的11-2是右子树。我们回到preorder中，10为左子树的根，2为右子树的根。再去preorder里面，10左侧的4为左子树，右侧的3、1为右子树。2往左的11和8为左子树。再递归，直至结束。

所以这里递归的，其实是preorder和inorder这两个数组的左右边界。取root，永远是preorder边界内的第一个元素。然后用root在inorder里的index，去更新下一递归inorder的范围。

preorder的范围如何确定？从root往后算长度，左子树长度的就是左子树范围，右子树长度的，就是右子树范围。

来源：https://www.cnblogs.com/NickyYe/p/4456782.html

C++版：

```C++
/*
Runtime: 24 ms, faster than 79.57% of C++ online submissions for Construct Binary Tree from Preorder and Inorder Traversal.
Memory Usage: 16.7 MB, less than 61.94% of C++ online submissions for Construct Binary Tree from Preorder and Inorder Traversal.
*/

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
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int prelen = preorder.size();
        int inlen = inorder.size();
        int prei=0,prej=inlen-1,ini=0,inj=inlen-1;
        //preorder:前序序列；inorder：中序序列
        //prei:前序序列起始位置；prej：前序序列结束位置
        //ini:中序序列起始位置；inj：中序序列结束位置
        return dfs(preorder,inorder,prei,prej,ini,inj);
    }
    TreeNode *dfs(vector<int>& preorder, vector<int>& inorder,int prei,int prej,int ini,int inj)
    {
        if(ini>inj || prei>prej)
            return NULL;
        TreeNode* node = new TreeNode(preorder[prei]);//根节点
        int index=0;
        for(int i=ini;i<=inj;i++)
        {
            if(inorder[i]==preorder[prei])
            {
                index=i;//左子树与右子树的分隔点
                break;
            }
        }
        //index-ini:以preorder[prei]为根节点的左子树的长度
        //prei+1：左子树的根节点；prei+index-ini：左子树在前序序列的终止位置
        node->left = dfs(preorder,inorder,prei+1,prei+index-ini,ini,index-1);//分隔点左边为左子树
        
        //prei+1+index-ini：右子树的根节点；prej：右子树在前序序列的终止位置        
        node->right = dfs(preorder,inorder,prei+1+index-ini,prej,index+1,inj);//分隔点右边为右子树
        return node;
    }
};
```

Python3版：

```python
'''
Runtime: 240 ms, faster than 18.50% of Python3 online submissions for Construct Binary Tree from Preorder and Inorder Traversal.
Memory Usage: 87.7 MB, less than 8.65% of Python3 online submissions for Construct Binary Tree from Preorder and Inorder Traversal.
'''

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        if len(preorder) == 0:
            return None
        note = TreeNode(preorder[0])
        index = inorder.index(preorder[0])
        note.left = self.buildTree(preorder[1:index+1],inorder[0:index])
        note.right = self.buildTree(preorder[index+1:],inorder[index+1:])
        return note
```

