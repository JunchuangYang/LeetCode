## [90. Subsets II](https://leetcode.com/problems/subsets-ii/)

Medium

Given a collection of integers that might contain duplicates, **nums**, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

**Example:**

```
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

这道题和[78.Subsets](https://leetcode.com/problems/subsets/)很相似，唯一区别是本题元素需要重复，需要判别重复元素。

我在78题的基础上改了代码，加了判别，但是一直wa，无奈只能依靠百度了。

参考：[LeetCode\] Subsets II 子集合之二](https://www.cnblogs.com/grandyang/p/4310964.html)

对于递归的解法，根据之前[ Subsets 子集合](http://www.cnblogs.com/grandyang/p/4309345.html) 里的构建树的方法，在处理到第二个2时，由于前面已经处理了一次2，这次我们只在添加过2的[2] 和 [1 2]后面添加2，其他的都不添加，那么这样构成的二叉树如下图所示：



```
                        []        
                   /          \        
                  /            \     
                 /              \
              [1]                []
           /       \           /    \
          /         \         /      \        
       [1 2]       [1]       [2]     []
      /     \     /   \     /   \    / \
  [1 2 2] [1 2]  X   [1]  [2 2] [2] X  []
```

 

代码只需在原有的基础上增加一句话，while (S[i] == S[i + 1]) ++i; 这句话的作用是跳过树中为X的叶节点，因为它们是重复的子集，应被抛弃。代码如下：



根据上面的思想，我的理解是：该开始添加时一条道走到黑，到开始回溯时判断当前元素是否与下一个元素相同，如果相同的话说明已经添加过了，则略去。

C++：

```c++
/*
Runtime: 12 ms, faster than 66.03% of C++ online submissions for Subsets II.
Memory Usage: 9.8 MB, less than 32.31% of C++ online submissions for Subsets II.
*/
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(),nums.end());
        vector<int> s;
        vector<vector<int>> v;
        v.push_back(s);
        dfs(nums,0,v,s);
        return v;
    }
    void dfs(vector<int> nums,int i,vector<vector<int>> &v,vector<int> s)
    {
        if(i>=nums.size()) return;
        for( int j=i;j<nums.size();j++)
        {
            s.push_back(nums[j]);
            dfs(nums,j+1,v,s);
            v.push_back(s);
            s.pop_back();
            while(j+1<nums.size()&&nums[j+1]==nums[j])//与78题不同的地方
                j++;
        }   
    }
};
```



 Python：

再用python语言实现上述思想的时候我出现了一个错误，**Python中迭代变量j在循环体中改变循环范围改变不了**，列如 

```python
for j in range(0,10):
    j = j + 1
这个时候j 还是从0~10进行循环，j值人工改变不了

所以在下面的代码我用while循环模拟for循环
```



```python
'''
Runtime: 68 ms, faster than 7.93% of Python3 online submissions for Subsets II.
Memory Usage: 13.3 MB, less than 5.47% of Python3 online submissions for Subsets II.
'''
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        nums.sort()
        res = []
        s = []
        res.append(s.copy())
        def dfs(nums,i,s,res):
            if i >= n:
                return
            j = i
            while j < n:
                s.append(nums[j])
                res.append(s.copy())
                dfs(nums,j+1,s,res)
                s.pop()
                while (j+1<n) and (nums[j]==nums[j+1]):
                    j=j+1
                j = j + 1
        dfs(nums,0,s,res)
        return res
    

                
```

