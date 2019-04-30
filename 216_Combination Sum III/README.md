## [216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)

Medium

Find all possible combinations of **\*k*** numbers that add up to a number **\*n***, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

**Note:**

- All numbers will be positive integers.
- The solution set must not contain duplicate combinations.

**Example 1:**

```
Input: k = 3, n = 7
Output: [[1,2,4]]
```

**Example 2:**

```
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]
```

题意：从1到9中挑选k个数使其等于目标值n；

DFS

C++版：

```c++
/*
Runtime: 8 ms, faster than 19.60% of C++ online submissions for Combination Sum III.
Memory Usage: 10.5 MB, less than 5.37% of C++ online submissions for Combination Sum III.
*/
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<int> sub;
        vector<vector<int>> res;
        int sum=0,num=0;
        return (dfs(res,sub,k,n,sum,num,1));
    }
    vector<vector<int>> dfs(vector<vector<int>> res,vector<int> sub,int k ,int n,int sum,int num,int i)
    {
        if(sum>n||num>k)
            return res;
        if (sum==n&&k==num)
        {
            res.push_back(sub);
            return res;
        }
        for(int j=i;j<=9;j++)
        {
            if(sum<n&&num<k)
            {
                sum+=j;
                sub.push_back(j);
                num++;
                res=dfs(res,sub,k,n,sum,num,j+1);
                sum-=j;
                num--;
                sub.pop_back();
            }
        }
        return res;
    }
};
```

上面是我自己首先写的，代码不够简洁，运行速度也不快。

参考：https://www.cnblogs.com/grandyang/p/4537983.html

n是k个数字之和，如果n小于0，则直接返回，如果n正好等于0，而且此时out中数字的个数正好为k，说明此时是一个正确解，将其存入结果res中.

```c++
/*
Runtime: 4 ms, faster than 100.00% of C++ online submissions for Combination Sum III.
Memory Usage: 8.7 MB, less than 38.93% of C++ online submissions for Combination Sum III.
*/
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<int> sub;
        vector<vector<int>> res;
        dfs(res,sub,k,n,1);
        return res;
    }
    void dfs(vector<vector<int>> &res,vector<int> &sub,int k,int n,int i)
    {
        if (sub.size()>k||n<0)
            return ;
        if (n==0&&sub.size()==k)
            res.push_back(sub);
        for(int j=i;j<=9;j++)
        {
            sub.push_back(j);
            dfs(res,sub,k,n-j,j+1);
            sub.pop_back();
        }
        return;
    }

};
```

Python3版：

```python
'''
Runtime: 40 ms, faster than 57.02% of Python3 online submissions for Combination Sum III.
Memory Usage: 13.3 MB, less than 5.68% of Python3 online submissions for Combination Sum III.
'''
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res = []
        sub = []
        def dfs(i,n):
            if len(sub)>k or n<0:
                return None;
            if len(sub)==k and n==0:
                res.append(sub.copy())
            for j in range(i,10):
                sub.append(j)
                dfs(j+1,n-j)
                sub.pop()
            return None
        dfs(1,n)
        return res
        
```

