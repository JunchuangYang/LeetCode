# [279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。

示例 1:

输入: n = 12
输出: 3 
解释: 12 = 4 + 4 + 4.
示例 2:

输入: n = 13
输出: 2
解释: 13 = 4 + 9.

**暴力：**

```python
class Solution(object):
    def numSquares(self, n):
        """
        :type n: int
        :rtype: int
        """
        # 1, 1 
        # 2, 1,1
        # 3, 1,1,1
        # 4, 4
        # 5, 4,1
        # 6, 4,1,1
        # 7, 4,1,1,1
        # 8, 4,4
        # 9, 9
        # 10, 9,1

        # dp[0]=0
        dp = [0 for _ in range(n+1)]
        p = []
        index = 1
        p.append(1)
        for i in range(1,n+1):
            if i == index**2:
                dp[i] = 1
                p.append(i)
                index+=1
            else:
                dp[i] = float('inf')
                for j in range(len(p)-1,(len(p)-1)//2,-1):
                    dp[i] = min(dp[i],dp[p[j]]*(i//p[j])+dp[i%p[j]])
                #print(iii)
        return dp[n]
```

