# [312. 戳气球](https://leetcode-cn.com/problems/burst-balloons/)

有 n 个气球，编号为0 到 n-1，每个气球上都标有一个数字，这些数字存在数组 nums 中。

现在要求你戳破所有的气球。如果你戳破气球 i ，就可以获得 nums[left] * nums[i] * nums[right] 个硬币。 这里的 left 和 right 代表和 i 相邻的两个气球的序号。注意当你戳破了气球 i 后，气球 left 和气球 right 就变成了相邻的气球。

求所能获得硬币的最大数量。

说明:

你可以假设 nums[-1] = nums[n] = 1，但注意它们不是真实存在的所以并不能被戳破。
0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100
示例:

```
输入: [3,1,5,8]
输出: 167 
解释: nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
     coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```

**记忆化搜索：**

以下方法超时

```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        if not nums:
            return 0
        self.memo = {}
        nums = [1]+nums[:]+[1]
        return self.dfs(nums,0)
    def dfs(self,nums,coins):
        if len(nums)==2:
            return coins
        if self.memo.get((tuple(nums),coins),-1)!=-1:
            return self.memo[(tuple(nums),coins)]
        res = float('-inf')
        for i in range(1,len(nums)-1):
            coins += nums[i]*nums[i-1]*nums[i+1]
            temp = nums[:]
            temp.pop(i)
            res = max(res,self.dfs(temp,coins))
            coins -= nums[i]*nums[i-1]*nums[i+1]
        self.memo[(tuple(nums),coins)] = res
        return res
```

------

[题解](<https://leetcode-cn.com/problems/burst-balloons/solution/chuo-qi-qiu-by-leetcode-solution/>)

看了题解之后换了一种方法进行记忆化搜索。通过，时间复杂度较高。

```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        if not nums:
            return 0
        self.memo = {}
        self.nums = [1]+nums[:]+[1]
        return self.dfs(0,len(self.nums)-1)
    def dfs(self,left,right):
        if left>=right:
            return 0
        if self.memo.get((left,right),-1)!=-1:
            return self.memo[(left,right)]
        res = 0
        for i in range(left+1,right):
            coins = self.dfs(left,i)+self.dfs(i,right)+self.nums[i]*self.nums[left]*self.nums[right]
            res = max(res,coins)
        self.memo[(left,right)] = res
        return res
```

**主流还得是动态规划**

```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        # dp[i][j]代表从i开始j结束的这一段开区间内最大coins数
        # dp[i][j] = dp[i][k] + dp[k][j] + nums[i]*nums[k]*nums[j]
        nums = [1]+nums[:]+[1]
        dp = [[0]*len(nums) for _ in range(len(nums))]
        # 从下至上,从左至右开始进行状态扫描
        # 因为如果计算dp[i][j]这个状态，必须计算dp[k][j]这个后面的状态，所以从下至上扫描
        for i in range(len(nums)-1,-1,-1):
            for j in range(1,len(nums)):
                for k in range(i+1,j):
                    dp[i][j] = max(dp[i][j],dp[i][k] + dp[k][j] + nums[i]*nums[k]*nums[j])
        return dp[0][len(nums)-1]

```

