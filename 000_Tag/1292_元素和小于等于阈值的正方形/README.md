# [1292. 元素和小于等于阈值的正方形的最大边长](https://leetcode-cn.com/problems/maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold/)

给你一个大小为 m x n 的矩阵 mat 和一个整数阈值 threshold。

请你返回元素总和小于或等于阈值的正方形区域的最大边长；如果没有这样的正方形区域，则返回 0 。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/12/15/e1.png)

```
示例 1：
输入：mat = [[1,1,3,2,4,3,2],[1,1,3,2,4,3,2],[1,1,3,2,4,3,2]], threshold = 4
输出：2
解释：总和小于 4 的正方形的最大边长为 2，如图所示。
示例 2：

输入：mat = [[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2],[2,2,2,2,2]], threshold = 1
输出：0
示例 3：

输入：mat = [[1,1,1,1],[1,0,0,0],[1,0,0,0],[1,0,0,0]], threshold = 6
输出：3
示例 4：

输入：mat = [[18,70],[61,1],[25,85],[14,40],[11,96],[97,96],[63,45]], threshold = 40184
输出：2

提示：

    1 <= m, n <= 300
    m == mat.length
    n == mat[i].length
    0 <= mat[i][j] <= 10000
    0 <= threshold <= 10^5
```

[官解](<https://leetcode-cn.com/problems/maximum-side-length-of-a-square-with-sum-less-than-or-equal-to-threshold/solution/yuan-su-he-xiao-yu-deng-yu-yu-zhi-de-zheng-fang-2/>):前缀和+二分

```python
class Solution:
    def maxSideLength(self, mat: List[List[int]], threshold: int) -> int:
        n,m = len(mat),len(mat[0])
        p = [[0]*(m+1) for _ in range(n+1)]#前缀和
        # 从p[1][1]开始
        for i in range(1,n+1):
            for j in range(1,m+1):
                p[i][j] = mat[i-1][j-1]+p[i-1][j]+p[i][j-1]-p[i-1][j-1]
        def check(mid):
            for i in range(mid,n+1):
                for j in range(mid,m+1):
                    if  p[i][j]-p[i-mid][j]-p[i][j-mid]+p[i-mid][j-mid]<=threshold:
                        return True
            return False
        l,r=0,min(n,m)
        ans = 0
        while l<=r:
            mid = l+(r-l)//2
            if check(mid):
                ans = mid
                l=mid+1
            else:
                r=mid-1
        return ans
```

