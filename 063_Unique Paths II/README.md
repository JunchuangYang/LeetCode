## [63.Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

Medium

A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

![img](https://github.com/JunchuangYang/LeetCode/blob/master/062_Unique%20Paths/robot_maze.png)

An obstacle and empty space is marked as `1` and `0` respectively in the grid.

**Note:** *m* and *n* will be at most 100.

**Example 1:**

```
Input:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
Output: 2
Explanation:
There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```

刚看到题目时想用深搜来写，可注意到m和n最大为100时发现深搜应该会超时，于是想起了动态规划。

简单版动态规划。

```
转移函数为dp[i][j]=dp[i-1][j]+dp[i][j-1]
dp[i][j]这个位置的次数是由他上方和左方的次数之和得到的，于是，只要稍微的判定一下边界的情况和在初始时设置障碍这个特殊情况就可以了。
还有注意dp这个数组要设置成long long 类型的，int会溢出。
```

C++版：

```c++
/*
Runtime: 4 ms, faster than 100.00% of C++ online submissions for Unique Paths II.
Memory Usage: 9.1 MB, less than 84.87% of C++ online submissions for Unique Paths II.
*/

class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int n = obstacleGrid.size(),m=obstacleGrid[0].size();
        long long  dp[101][101]={0};
        if(obstacleGrid[0][0])
            return 0;
        dp[0][0]=1;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if(i==0&j==0)
                    continue;
                else if(obstacleGrid[i][j])
                    dp[i][j]=0;
                else if(i==0)
                    dp[i][j]=dp[i][j-1];
                else if(j==0)
                    dp[i][j]=dp[i-1][j];
                else
                    dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[n-1][m-1];
    }
};
```

Python3版：

思想同上。

我要特别强调一下用Python创建二维数组的方式

参考博客：[python的二维数组操作](https://www.cnblogs.com/btchenguang/archive/2012/01/30/2332479.html)

```python
'''
Runtime: 40 ms, faster than 61.60% of Python3 online submissions for Unique Paths II.
Memory Usage: 13.3 MB, less than 5.19% of Python3 online submissions for Unique Paths II.
'''
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        n = len(obstacleGrid)
        m = len(obstacleGrid[0])
        dp = [[0 for i in range(m)]for j in range(n)] #Python二维数组创建方式
        if obstacleGrid[0][0]==1:
            return 0
        dp[0][0]=1
        for i in range(n):
            for j in range(m):
                if (i==0 and j==0) or (obstacleGrid[i][j]==1):
                    continue
                if i==0:
                    dp[i][j]=dp[i][j-1]
                elif j==0:
                    dp[i][j]=dp[i-1][j]
                else:
                    dp[i][j]=dp[i-1][j]+dp[i][j-1]
        return dp[n-1][m-1]
        
```

