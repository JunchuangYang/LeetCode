# [410. 分割数组的最大值](https://leetcode-cn.com/problems/split-array-largest-sum/)

给定一个非负整数数组和一个整数 m，你需要将这个数组分成 m 个非空的连续子数组。设计一个算法使得这 m 个子数组各自和的最大值最小。

注意:
数组长度 n 满足以下条件:

1 ≤ n ≤ 1000
1 ≤ m ≤ min(50, n)

```python
示例:

输入:
nums = [7,2,5,10,8]
m = 2

输出:
18

解释:
一共有四种方法将nums分割为2个子数组。
其中最好的方式是将其分为[7,2,5] 和 [10,8]，
因为此时这两个子数组各自的和的最大值为18，在所有情况中最小。
```

**[题解](<https://leetcode-cn.com/problems/split-array-largest-sum/solution/fen-ge-shu-zu-de-zui-da-zhi-by-leetcode-solution/>)**

**动态规划：**

hard：具体动态转移方程看题解。

```python
class Solution(object):
    def splitArray(self, nums, m):
        """
        :type nums: List[int]
        :type m: int
        :rtype: int
        """
        #本题中，我们可以令 f[i][j] 表示将数组的前 i 个数分割为 j 段所能得到的最大连续子数组和的最小值
        n = len(nums)
        dp = [[float('inf')]*(m+1) for _ in range(n+1)]
        sub = [0]*(n+1)
        dp[0][0]=0
        for i in range(1,n+1):
            sub[i]+=sub[i-1]+nums[i-1]
        for i in range(1,n+1):
            for j in range(1,min(i+1,m+1)):
                for k in range(i):
                    dp[i][j] = min(max(dp[k][j-1],sub[i]-sub[k]),dp[i][j])
        return dp[n][m]

```

**二分**

思路及算法

**「使……最大值尽可能小」是二分搜索题目常见的问法。**

本题中，我们注意到：当我们选定一个值 xx，我们可**以线性地验证是否存在一种分割方案**，满足其最大分割子数组和不超过 xx。策略如下：

贪心地模拟分割的过程，从前到后遍历数组，用 total 表示当前分割子数组的和，n 表示已经分割出的子数组的数量（包括当前子数组），那么每当 total 加上当前值超过了 mid，我们就把当前取的值作为新的一段分割子数组的开头，并将 n 加 1。遍历结束后验证是否 n 不超过 m。

这样我们可以用二分查找来解决。**二分的上界为数组 nums 中所有元素的和，下界为数组 nums 中所有元素的最大值。通过二分查找，我们可以得到最小的最大分割子数组和，这样就可以得到最终的答案了。**

```python
class Solution(object):
    def splitArray(self, nums, m):
        """
        :type nums: List[int]
        :type m: int
        :rtype: int
        """
        left = max(nums)
        right = sum(nums)
        while left<right:
            mid = left + (right-left)//2
            if self.check(mid,nums,m):
                right = mid
            else:
                left = mid+1
        return left
    
    def check(self,mid,nums,m):
        total,n = 0,1
        for item in nums:
            if total+item>mid:
                total = item
                n+=1
            else:
                total+=item
        return n<=m
```

