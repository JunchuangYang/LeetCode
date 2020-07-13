# [85. 最大矩形](https://leetcode-cn.com/problems/maximal-rectangle/)

给定一个仅包含 0 和 1 的二维二进制矩阵，找出只包含 1 的最大矩形，并返回其面积。

```
示例:

输入:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
输出: 6			
```

**动态规划：边统计边压缩**：超时

```python
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        if not matrix:
            return 0
        n,m = len(matrix),len(matrix[0])
        dp = [[0]*m for _ in range(n)]
        res = 0
        num = 0
        for i in range(0,n):
            num += 1
            for j in range(i,n):
                flag = 0
                for k in range(0,m):
                    if matrix[j][k]=='1':
                        flag+=1
                    else:
                        flag=0
                    dp[j][k]=flag
                    res = max(res,dp[j][k]*num)
            # 压缩
            for j in range(n-1,i,-1):
                for k in range(0,m):
                    matrix[j][k] = str(int(matrix[j][k]) & int(matrix[j-1][k]))
        return res 
```



**动态规划：使用柱状图的优化暴力方法**：[题解](<https://leetcode-cn.com/problems/maximal-rectangle/solution/zui-da-ju-xing-by-leetcode/>)

```python
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        if not matrix:
            return 0
        n,m = len(matrix),len(matrix[0])
        dp = [[0]*m for _ in range(n)]
        res = 0
        for i in range(0,n):
            for j in range(0,m):
                if matrix[i][j]=='0':
                    continue
                dp[i][j]=1
                if j-1>=0:
                    dp[i][j]+=dp[i][j-1]
                min_len = float('inf')
                for k in range(i,-1,-1):
                    min_len = min(min_len,dp[k][j])
                    res = max(res,(i-k+1)*min_len)
        #print(dp)
        return res
```

**动态规划：柱状图+单调栈。题解同上**

```python
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        if not matrix:
            return 0
        n,m = len(matrix),len(matrix[0])
        dp = [0]*m
        res = 0
        for i in range(0,n):
            for j in range(0,m):
                if matrix[i][j] == '1':
                    dp[j]+=1
                else:
                    dp[j]=0
            res = max(res,self.monoStack(dp))
        return res
    # 从84： 柱状图中最大的矩形的代码中复制过来
    def monoStack(self,heights):
        # 单调栈
        # 其中：left保存当前i左边最近的小于heights[i]的高度
        #       right保存当前i右边最近的小于heights[i]的高度
        n = len(heights)
        left,right = [0]*n,[0]*n
        mono_stack = []
        for i in range(n):
            while mono_stack and heights[mono_stack[-1]]>=heights[i]:
                mono_stack.pop()
            # 将-1作为哨兵
            left[i] = mono_stack[-1] if mono_stack else -1
            mono_stack.append(i)
        mono_stack = []
        for i in range(n-1,-1,-1):
            while mono_stack and heights[mono_stack[-1]]>=heights[i]:
                mono_stack.pop()
            # 将n作为哨兵
            right[i] = mono_stack[-1] if mono_stack else n
            mono_stack.append(i)
        res = max((right[i]-left[i]-1)*heights[i] for i in range(n)) if n>0 else 0
        return res
```

