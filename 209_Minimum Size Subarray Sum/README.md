## [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

Medium

Given an array of **n** positive integers and a positive integer **s**, find the minimal length of a **contiguous** subarray of which the sum ≥ **s**. If there isn't one, return 0 instead.

**Example:** 

```
Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: the subarray [4,3] has the minimal length under the problem constraint.
```

**Follow up:**

If you have figured out the *O*(*n*) solution, try coding another solution of which the time complexity is *O*(*n* log *n*). 

题意：从给定序列中找出子序列的和大于或等于目标值的最小序列长度。

C++版：

用数组dp[i]将i前面的元素加起来，如果超过了给定的目标值，则用index=0，从序列初始位置开始从dp[i]-nums[index]，如果减去nums[index]后值仍然超过目标值，则更新长度，继续迭代。



也就是说，dp[i]的表示意思是，前i个连续元素中，最接近目标值的元素个数。

```c++
/*
Runtime: 12 ms, faster than 98.50% of C++ online submissions for Minimum Size Subarray Sum.
Memory Usage: 10.1 MB, less than 14.41% of C++ online submissions for Minimum Size Subarray Sum.
*/
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int n = nums.size();
        if(n==0) return 0;
        int dp[n]={0},res=INT_MAX,num=1,index=0;
        dp[0] = nums[0];
        if(dp[0]>=s)
            res = 1;
        for (int i=1;i<n;i++)
        {
            dp[i] = dp[i-1]+nums[i];
            num++;
            if(dp[i]>=s)
            {
                res=min(res,num);
                while(dp[i]>=s&&index<=i)
                {
                    res=min(res,num);
                    dp[i]-=nums[index];
                    num-=1;
                    index++;
                }
            
            }
        }
        return res==INT_MAX?0:res;
    }
};
```

Python3：

其实思想和c++的一样，这里我没有去开辟list，而是使用了双指针，for循环中i算为右指针，index为左指针，保证index~i之间的和小于s，如果大于s，则减去nums[index],移动左指针index+1。

```python
'''
Runtime: 48 ms, faster than 85.37% of Python3 online submissions for Minimum Size Subarray Sum.
Memory Usage: 14.6 MB, less than 12.78% of Python3 online submissions for Minimum Size Subarray Sum.
'''
class Solution:
    def minSubArrayLen(self, s: int, nums: List[int]) -> int:
        n = len(nums)
        if n==0:
            return 0
        if nums[0] >=s:
            return 1
        summ = nums[0]
        flag = 1
        res = n + 1
        index = 0
        for i in range(1,n):
            summ += nums[i]
            flag += 1
            if summ >= s:
                while summ>=s and index<=i:
                    res = min(res,flag)
                    summ -= nums[index]
                    flag -= 1
                    index += 1
        if res == n+1:
            return 0
        else:
            return res
        
```

