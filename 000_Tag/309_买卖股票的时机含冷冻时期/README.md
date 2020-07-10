# [309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

给定一个整数数组，其中第 i 个元素代表了第 i 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。
示例:

输入: [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]

[官解](<https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/solution/zui-jia-mai-mai-gu-piao-shi-ji-han-leng-dong-qi-4/>)

#### 动态规划

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0
        
        n = len(prices)
        # dp[i][0]: 手上持有股票的最大收益
        # dp[i][1]: 手上不持有股票，并且处于冷冻期中的累计最大收益
        # dp[i][2]: 手上不持有股票，并且不在冷冻期中的累计最大收益
        dp = [[0,0,0] for _ in range(len(prices))]
        dp[0][0] = -prices[0] # 买入
        for i in range(1,n):
            dp[i][0] = max(dp[i-1][0],dp[i-1][2]-prices[i])
            dp[i][1] = dp[i-1][0]+prices[i]
            dp[i][2] = max(dp[i-1][2],dp[i-1][1])
        return max(dp[n-1][0],dp[n-1][1],dp[n-1][2])
```

