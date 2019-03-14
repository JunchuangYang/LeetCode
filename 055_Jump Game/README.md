## [55. Jump Game](https://leetcode.com/problems/jump-game/)

Medium

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

**Example 1:**

```
Input: [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum
             jump length is 0, which makes it impossible to reach the last index.
```

思想与[45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)差不多，是它的简化版本。

C++版：

```c++
/*
用的是和45题相同的贪心算法。
Runtime: 12 ms
*/
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int len = nums.size();
        int maxpos = nums[0];
        int curpos = nums[0];
        for (int i = 1 ;i<=min(len-1,maxpos);i++ )
        {
            curpos = max(curpos,nums[i]+i);
            if(i==maxpos)
            {
                maxpos = curpos;
            }
        }
        if (maxpos>=len-1) return true;
        else return false;
    }
};
```

这里参考别的博客写了一个简单的一维的动态规划版本，运行时间与上述代码相差很多。

```c++
/*
动态规划版本：dp[i]表示从0走到i最少要用的的步数。
Runtime: 2448 ms
*/
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int len = nums.size();
        int dp[100000]={INT_MAX};
        
        for(int i=0;i<len;i++)
            dp[i] = INT_MAX-1;
        dp[0]=0;
        for(int i=1;i<len;i++)
        {
            for(int j=0;j<i;j++)
            {
                if(nums[j]+j>=i)
                    dp[i]=min(dp[i],dp[j]+1);
            }
        }
        if(dp[len-1]>len) return false;
        else return true;
    }
};
```

