[95. Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)

Medium

Given an integer *n*, generate all structurally unique **BST's** (binary search trees) that store values 1 ... *n*.

**Example:**

```
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
Explanation:
The above output corresponds to the 5 unique BST's shown below:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

一直没看懂，先放一放。

https://segmentfault.com/a/1190000007443961

https://blog.csdn.net/caoyan_12727/article/details/54348749

---

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
	 vector<TreeNode*> generateTrees(int n) {
		 vector<TreeNode *>ret;
		 if (n == 0)
             return ret;
		 return dfs(n,1);
	 }
     vector<TreeNode *> dfs(int n , int val_start)
     {
         vector<TreeNode *> ret;
         //判断为空节点
         if(n==0)
         {
             ret.push_back(NULL);
             return ret;
         }
         //判断为叶节点
         if(n==1)
         {
             ret.push_back(new TreeNode(val_start));
             return ret;
         }
         for(int i=0;i<n;i++)
         {
             vector<TreeNode *> left = dfs(i,val_start);
             vector<TreeNode *> right = dfs(n-i-1,val_start+i+1);
             for(int l=0;l<left.size();l++)
             {
                 for(int r=0;r<right.size();r++)
                 {
                     TreeNode* root=new TreeNode(val_start+i);
                     root->left=left[l];
                     root->right=right[r];
                     ret.push_back(root);
                 }
             }
         }
          return ret;   
     }
        
 };

```

