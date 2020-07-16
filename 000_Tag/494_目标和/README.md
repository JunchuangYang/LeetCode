# [494. 目标和](https://leetcode-cn.com/problems/target-sum/)

给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 + 和 -。对于数组中的任意一个整数，你都可以从 + 或 -中选择一个符号添加在前面。

返回可以使最终数组和为目标数 S 的所有添加符号的方法数。

 ```python
示例：

输入：nums: [1, 1, 1, 1, 1], S: 3
输出：5
解释：

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

一共有5种方法让最终目标和为3。
 

提示：

数组非空，且长度不会超过 20 。
初始的数组的和不会超过 1000 。
保证返回的最终结果能被 32 位整数存下。
 ```

**深搜+剪枝：记忆化搜索**

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], S: int) -> int:
        self.dic = {}
        return self.dfs(nums,0,0,S)
    def dfs(self,nums,index,sums,S):
        if len(nums) == index:
            if sums == S:
                return 1
            return 0
        if (index,sums) in self.dic:
            return self.dic[(index,sums)]
        ans = self.dfs(nums,index+1,sums+nums[index],S)+self.dfs(nums,index+1,sums-nums[index],S)
        self.dic[(index,sums)] = ans
        return ans
```

**二维dp:这个动态规划还没有深搜快**

**递推方程**：dp\[i][j] =  dp\[i-1][j-nums[i-1]] + dp\[i-1][j+nums[i-1]] # dp\[i][j]:代表以num[i]为结尾的前i个数和为j的个数

需要控制一下索引为负数的情况

**题解里还有用01背包解法，需要推一下**



```python
class Solution:
    def findTargetSumWays(self, nums: List[int], S: int) -> int:
        # dp[i][j]:代表以num[i]为结尾的前i个数和为j的个数
        val = sum(nums)
        if val<S:
            return 0
        dp = [[0]*(val*2+1) for _ in range(len(nums))]
        dp[0][nums[0]+val] += 1
        dp[0][-nums[0]+val] += 1
        for i in range(1,len(nums)):
            for j in range(0,val*2+1):
                if 0<=j-nums[i]<=val*2:
                    dp[i][j] += dp[i-1][j-nums[i]] 
                if 0<=j+nums[i]<=val*2:    
                    dp[i][j] += dp[i-1][j+nums[i]] 
        return dp[len(nums)-1][S+val]
```

