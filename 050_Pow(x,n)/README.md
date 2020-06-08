# [50. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)

实现 pow(x, n) ，即计算 x 的 n 次幂函数。

示例 1:

输入: 2.00000, 10
输出: 1024.00000
示例 2:

输入: 2.10000, 3
输出: 9.26100
示例 3:

输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25
说明:

-100.0 < x < 100.0
n 是 32 位有符号整数，其数值范围是 [−2^31, 2^31 − 1] 。

**思路：**

我用的暴力方法超时，官方使用的递归。还有一种方法使用**快速幂**。

<https://leetcode-cn.com/problems/powx-n/solution/powx-n-by-leetcode-solution/>

```python
class Solution(object):
    def myPow(self, x, n):
        """
        :type x: float
        :type n: int
        :rtype: float
        """
        return 1.0/self.dfs(x,abs(n)) if n < 0 else self.dfs(x,abs(n))
    def dfs(self,x,n):
        if n==0:
            return 1.0
        y = self.dfs(x,n//2)
        return y*y if n%2==0 else y*y*x
```

