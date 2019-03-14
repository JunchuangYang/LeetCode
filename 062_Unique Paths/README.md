## [62.Unique Paths II](https://leetcode.com/problems/unique-paths/)

Medium

A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

![img](/robot_maze.png)

Above is a 7 x 3 grid. How many possible unique paths are there?

**Note:** *m* and *n* will be at most 100.

**Example 1:**

```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
```

**Example 2:**

```
Input: m = 7, n = 3
Output: 28
```

由于我是先做的[63.Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)，这两题其实思想一样，63比62多设了一个障碍，在63中将障碍判断的代码删了就是62的解。

以下内容与63大致相同:

刚看到题目时想用深搜来写，可注意到m和n最大为100时发现深搜应该会超时，于是想起了动态规划。

简单版动态规划。

```
转移函数为dp[i][j]=dp[i-1][j]+dp[i][j-1]
dp[i][j]这个位置的次数是由他上方和左方的次数之和得到的
还有注意dp这个数组要设置成long long 类型的，int会溢出。
```

C++版：

```c++
/*
Runtime: 4 ms, faster than 100.00% of C++ online submissions for Unique Paths.
Memory Usage: 8.3 MB, less than 64.10% of C++ online submissions for Unique Paths.
*/

class Solution {
public:
    int uniquePaths(int n, int m) {
        long long  dp[101][101]={0};
        dp[0][0]=1;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if(i==0&j==0)
                    continue;
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

