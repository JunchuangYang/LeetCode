## [120. Triangle](https://leetcode.com/problems/triangle/)

Medium

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle

```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```

The minimum path sum from top to bottom is `11` (i.e., **2** + **3** + **5** + **1** = 11).

**Note:**

Bonus point if you are able to do this using only *O*(*n*) extra space, where *n* is the total number of rows in the triangle.

动态规划：从上往下走最短路径是多少？每一步只能走自己的邻近边。

C++版：

采用**自上而下**进行更新。

```c++
/*
Runtime: 8 ms, faster than 99.50% of C++ online submissions for Triangle.
Memory Usage: 10.1 MB, less than 40.22% of C++ online submissions for Triangle.
*/
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        int dp[n][n]={0};
        dp[0][0]=triangle[0][0];
        for(int i=1;i<n;i++)
        {
            for(int j=0;j<triangle[i].size();j++)
            {
                if(j==0)
                    dp[i][j]=dp[i-1][j]+triangle[i][j];
                else if (j==triangle[i].size()-1)
                    dp[i][j]=dp[i-1][j-1]+triangle[i][j];
                else
                    dp[i][j]=min(dp[i-1][j-1],dp[i-1][j])+triangle[i][j];
            }
        }
        int res=INT_MAX;
        for(int i=0;i<triangle[n-1].size();i++)
        {
            res=min(res,dp[n-1][i]);
        }
        return res;
    }
};
```

采用**自下而上**进行更新。

由于使用了一维数组，在内存上比上面的方法节省了空间。

时间复杂度倒是没有啥太大的变化。

参考：https://www.cnblogs.com/TenosDoIt/p/3436532.html

```c++

分析：从最小面一层开始往上计算，设dp[i][j]是以第i层j个元素为起点的最小路径和，动态规划方程如下

dp[i][j] = value[i][j] + max{dp[i-1][j], dp[i-1][j+1]}



因为每一层之和它下一层的值有关，因此只需要一个一位数组保存下层的值，代码如下： 
/*
Runtime: 8 ms, faster than 99.50% of C++ online submissions for Triangle.
Memory Usage: 9.8 MB, less than 96.09% of C++ online submissions for Triangle.
*/
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        int dp[n];
        for (int i=0;i<n;i++)
            dp[i] = triangle[n-1][i];
        for(int i=n-2;i>=0;i--)
        {
            for(int j=0;j<triangle[i].size();j++)
                dp[j] = min(dp[j],dp[j+1])+triangle[i][j]; 
        }

        return dp[0];
    }
};
```

Python3版：

```python
'''
Runtime: 56 ms, faster than 24.81% of Python3 online submissions for Triangle.
Memory Usage: 13.7 MB, less than 15.44% of Python3 online submissions for Triangle.
'''
class Solution:
    def minimumTotal(self, triangle: List[List[int]]) -> int:
        dp = []
        n = len(triangle)
        for i in range(n):
            dp.append(triangle[n-1][i])
        for i in range(n-2,-1,-1):
            for j in range(len(triangle[i])):
                dp[j] = min(dp[j],dp[j+1]) + triangle[i][j]
        return dp[0]
        
```

