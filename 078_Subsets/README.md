## [78.Subsets](https://leetcode.com/problems/subsets/)

Medium

Given a set of **distinct** integers, *nums*, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

**Example:**

```
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

深搜，找出一个序列中的所有子序列。

C++版：

```c++
/*
Runtime: 8 ms, faster than 100.00% of C++ online submissions for Subsets.
Memory Usage: 9.5 MB, less than 29.47%  of C++ online submissions for Subsets.
*/
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size();
        int flag[n]={0};
        vector<int> s;
        vector<vector<int>> v;
        v.push_back(s);
        dfs(nums,0,v,s,flag);
        return v;
    }
    void dfs(vector<int> nums,int i,vector<vector<int>> &v,vector<int> s,int flag1[])
    {
        if(i>=nums.size()) return;
        for( int j=i;j<nums.size();j++)
        {
            if(!flag1[j])
            {
                flag1[j]=1;
                s.push_back(nums[j]);
                dfs(nums,j+1,v,s,flag1);
                flag1[j]=0;
                v.push_back(s);
                s.pop_back();
            }
        }   
    }
};
```

第二种方法：位操作（参考：[LeetCode78:Subsets](https://blog.csdn.net/u012501459/article/details/46777141)）

对于数组[1,2,3]，可以用一个下标0和1表示是否选择该数字，0表示未选择，1表示选中，那么每一组3个0和1的组合表示一种选择，3位共有8种选择，分别是： 
000 ：对应[]   
001：对应[3]   
010 ：对应[2]   
011 ：对应[2,3]  
100 …  
101  
110  
111  
那么上面为1的位表示数组中该位被选中。 

那么只需要遍历0到1<< length中的数，判断每一个数中有那几位为1，为1的那几位即会构成一个子集中的一个元素。

```C++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int length=nums.size();
        sort(nums.begin(),nums.end());
        vector<vector<int> > result;
        for(int i=0;i<1<<length;i++)
        {
            vector<int> tmp;
            //计算i中有那几位为1
            for(int j=0;j<length;j++)
            {
                //判断i中第j位是否为1
                //1左移j位其第几位就是1，&操作是只有1&1才是1。a[i]&(1<<j)这样就是保留a[i]第j位的值,其他位都是0。这里的if判断a[i]&(1<<j)是0则输出0，不是输出1。
                if(i&1<<j)
                {
                    tmp.push_back(nums[j]);
                }
            }
            result.push_back(tmp);
        }
        return result;
    }


};
--------------------- 
作者：vincent-xia 
来源：CSDN 
原文：https://blog.csdn.net/u012501459/article/details/46777141 
版权声明：本文为博主原创文章，转载请附上博文链接！
```

Python3:

```python
'''
Runtime: 44 ms, faster than 60.71% of Python3 online submissions for Subsets.
Memory Usage: 13.1 MB, less than 5.00% of Python3 online submissions for Subsets.
'''
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        res = []
        for i in range(1<<n):
            r = []
            for j in range(n):
                if i&(1<<j):
                    r.append(nums[j])
            res.append(r)
        return res
        
```

