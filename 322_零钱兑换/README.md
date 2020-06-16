# [322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

示例 1:

输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
示例 2:

输入: coins = [2], amount = 3
输出: -1


说明:
你可以认为每种硬币的数量是无限的。

**动态规划：**

```python
class Solution(object):
    def coinChange(self, coins, amount):
        """
        :type coins: List[int]
        :type amount: int
        :rtype: int
        """
        if amount == 0:
            return 0
        dp = [amount+1 for _ in range(amount+1)]
        for i in range(len(coins)):
            if amount>=coins[i]:
                dp[coins[i]] = 1
        for i in range(amount+1):
            for j in range(len(coins)):
                if i>coins[j]:
                    dp[i] = min(dp[i],dp[i-coins[j]]+1) 
        return -1 if dp[amount] == amount+1 else dp[amount]
```

