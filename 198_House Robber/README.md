### [198. House Robber](https://leetcode.com/problems/house-robber/)

Easy

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

**Example 1:**

```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

**Example 2:**

```
Input: [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
             Total amount you can rob = 2 + 9 + 1 = 12.
```

动态规划：相邻的两个位置不能同时选取。

dp[i] = max(dp[i-2]+nums[i],dp[i-1])

C++

```c++
/*
Runtime: 4 ms, faster than 93.54% of C++ online submissions for House Robber.
Memory Usage: 8.6 MB, less than 78.37% of C++ online submissions for House Robber.
*/
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if (n==0)
            return 0;
        if(n==1)
            return nums[0];
        int dp[n]={0};
        dp[0] = nums[0];
        dp[1] = max(nums[1],nums[0]);
        for(int i =2;i<n;i++)
        {
            dp[i] = max(dp[i-2]+nums[i],dp[i-1]);
        }

        return dp[n-1];
    }
};
```

Python3:

```python
'''
Runtime: 32 ms, faster than 96.59% of Python3 online submissions for House Robber.
Memory Usage: 13.2 MB, less than 29.91% of Python3 online submissions for House Robber.
'''
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        if n==0:
            return 0
        if n==1:
            return nums[0]
        nums[1] = max(nums[0],nums[1])
        for i in range(2,n):
            nums[i] = max(nums[i-2]+nums[i],nums[i-1])
        return nums[-1]
```



### [213. House Robber II](https://leetcode.com/problems/house-robber-ii/)

Medium

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.**That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

**Example 1:**

```
Input: [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2),
             because they are adjacent houses.
```

**Example 2:**

```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

**给定一个list,代表一条街道上每栋房子里的财物.我们要尽可能多地抢这些财物,但是不能抢相邻的两栋房子.这个街道是环形的.**

**这题和198很像,区别只是增加了一个第一栋房子与最后一栋房子不能同时抢的判定.所以我们分为两种情况:**

1. **抢了第一栋房子,此时问题变为198题的求0~N-1**

2. **没有抢第一栋房子,此时问题变为198题的求1~N**

   参考链接

https://www.cnblogs.com/limitlessun/p/8530277.html

C++

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if(n==0)
            return 0;
        if(n==1)
            return nums[0];
        if(n==2)
            return max(nums[0],nums[1]);
        int dp[n]={0};
        dp[0]=nums[0];
        dp[1]=max(nums[0],nums[1]);
        int m;
        for(int i=2;i<n-1;i++)
        {
            dp[i] = max(dp[i-1],dp[i-2]+nums[i]);
        }
        m = dp[n-2];
        dp[1]=nums[1];
        dp[2]=max(nums[1],nums[2]);
        for(int i=3;i<n;i++)
        {
            dp[i] = max(dp[i-1],dp[i-2]+nums[i]);
        }
        return max(m,dp[n-1]);
        
    }
};
```

Python3

```python
'''
Runtime: 36 ms, faster than 86.90% of Python3 online submissions for House Robber II.
Memory Usage: 13 MB, less than 78.10% of Python3 online submissions for House Robber II.
'''
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        num = nums.copy()
        if n==0:
            return 0
        if n==1:
            return nums[0]
        if n==2:
            return max(nums[0],nums[1])
        nums[1] = max(nums[0],nums[1])
        for i in range(2,n-1):
            nums[i] = max(nums[i-2]+nums[i],nums[i-1])
        num[2] = max(num[1],num[2])
        for i in range(3,n):
            num[i] = max(num[i-2]+num[i],num[i-1])
        return max(nums[-2],num[-1])
        
```

