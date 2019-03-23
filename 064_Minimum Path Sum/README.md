## [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

Given a *m* x *n* grid filled with non-negative numbers, find a path from top left to bottom right which *minimizes* the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

**Example:**

```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```

思想：简单版动态规划，同62,63题

转移函数：

```c++
dp[i][j]=grid[i][j]+min(dp[i-1][j],dp[i][j-1])
```

C++版：

```c++
/*
Runtime: 12 ms, faster than 99.16% of C++ online submissions for Minimum Path Sum.
Memory Usage: 10.8 MB, less than 91.51% of C++ online submissions for Minimum Path Sum.
*/
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();
        int dp[n][m];
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if(i==0&&j==0)
                    dp[0][0]=grid[0][0];
                else if (i==0)
                    dp[i][j]=grid[i][j]+dp[i][j-1];
                else if(j==0)
                    dp[i][j]=grid[i][j]+dp[i-1][j];
                else
                    dp[i][j]=grid[i][j]+min(dp[i][j-1],dp[i-1][j]);
            }
        }
        return dp[n-1][m-1];
    }
};
```

Python版：

```python
'''
Runtime: 60 ms, faster than 47.64% of Python3 online submissions for Minimum Path Sum.
Memory Usage: 14.5 MB, less than 10.80% of Python3 online submissions for Minimum Path Sum.
'''
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        n = len(grid)
        m = len(grid[0])
        dp = [[0 for i in range(m)] for j in range(n)]
        
        for i in range(n):
            for j in range(m):
                if i==0 and j==0:
                    dp[0][0] = grid[0][0]
                elif i==0:
                    dp[i][j] = grid[i][j] + dp[i][j-1]
                elif j==0:
                    dp[i][j] = grid[i][j] + dp[i-1][j]
                else:
                    dp[i][j] = grid[i][j] + min(dp[i-1][j],dp[i][j-1])
        return dp[n-1][m-1]
                    
```

