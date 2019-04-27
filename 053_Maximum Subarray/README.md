## [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

Easy

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Example:**

```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

**Follow up:**

If you have figured out the O(*n*) solution, try coding another solution using the divide and conquer approach, which is more subtle.



参考：https://www.cnblogs.com/bakari/p/4007368.html

用动态规划的方法，就是要找到其转移方程式，也叫动态规划的递推式，动态规划的解法无非是维护两个变量，局部最优和全局最优，我们先来看Maximum SubArray的情况，如果遇到负数，相加之后的值肯定比原值小，但可能比当前值大，也可能小，所以，对于相加的情况，只要能够处理局部最大和全局最大之间的关系即可，对此，写出转移方程式如下：
local[i + 1] = Max(local[i] + A[i], A[i]);

global[i + 1] = Max(local[i + 1], global[i]);

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        int dp[n]={0},res=-INT_MAX;
        dp[0] = nums[0];
        res = max(dp[0],res);
        for(int i=1;i<n;i++)
        {
            dp[i]=max(nums[i],dp[i-1]+nums[i]);
            res = max(res,dp[i]);
        }
        return res;
        
    }
};
```

## [152. Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

Medium

Given an integer array `nums`, find the contiguous subarray within an array (containing at least one number) which has the largest product.

**Example 1:**

```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```

**Example 2:**

```
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

C++版：

而对于Product Subarray，要考虑到一种特殊情况，即负数和负数相乘：如果前面得到一个较小的负数，和后面一个较大的负数相乘，得到的反而是一个较大的数，如{2，-3，-7}，所以，我们在处理乘法的时候，除了需要维护一个局部最大值，同时还要维护一个局部最小值，由此，可以写出如下的转移方程式：

```c++

max_copy[i] = max_local[i]
max_local[i + 1] = Max(Max(max_local[i] * A[i], A[i]),  min_local * A[i])

min_local[i + 1] = Min(Min(max_copy[i] * A[i], A[i]),  min_local * A[i])
```





```c++
/*
Runtime: 8 ms, faster than 70.97% of C++ online submissions for Maximum Product Subarray.
Memory Usage: 9.3 MB, less than 9.37% of C++ online submissions for Maximum Product Subarray.
Next challenges:
*/
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int n = nums.size();
        int res=nums[0];
        int dpmin[n]={0},dpmax[n]={0};
        dpmin[0]=dpmax[0]=nums[0];
        
        for(int i=1;i<n;i++)
        {
            dpmax[i] = max(max(nums[i],dpmax[i-1]*nums[i]),dpmin[i-1]*nums[i]);
            dpmin[i] = min(min(nums[i],dpmin[i-1]*nums[i]),dpmax[i-1]*nums[i]);
            res = max(max(dpmax[i],dpmin[i]),res);
        }
        
        return res;
    }
};
```

Python3版：

```python
'''
Runtime: 60 ms, faster than 22.09% of Python3 online submissions for Maximum Product Subarray.
Memory Usage: 14.5 MB, less than 5.52% of Python3 online submissions for Maximum Product Subarray.
'''

class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        n = len(nums)
        dpmin = []
        dpmax = []
        res = nums[0]
        
        dpmin.append(nums[0])
        dpmax.append(nums[0])
        
        for i in range(1,n):
            dpmax.append(max(max(nums[i],dpmax[i-1]*nums[i]),dpmin[i-1]*nums[i]))
            dpmin.append(min(min(nums[i],dpmin[i-1]*nums[i]),dpmax[i-1]*nums[i]))
            res = max(max(dpmax[i],dpmin[i]),res)
        return res
```

