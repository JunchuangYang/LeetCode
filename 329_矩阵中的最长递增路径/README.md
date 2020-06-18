# [329. 矩阵中的最长递增路径](https://leetcode-cn.com/problems/longest-increasing-path-in-a-matrix/)

给定一个整数矩阵，找出最长递增路径的长度。

对于每个单元格，你可以往上，下，左，右四个方向移动。 你不能在对角线方向上移动或移动到边界外（即不允许环绕）。

```
示例 1:

输入: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
输出: 4 
解释: 最长递增路径为 [1, 2, 6, 9]。
示例 2:

输入: nums = 
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
] 
输出: 4 
解释: 最长递增路径是 [3, 4, 5, 6]。注意不允许在对角线方向上移动。
```

**普通搜索：超时**

```python
class Solution(object):
    def longestIncreasingPath(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: int
        """
        self.length = 0
        self.route = []
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                if [i,j] not in self.route:
                    self.dfs(i,j,matrix,[],[])
        return self.length
    
    def dfs(self,x,y,matrix,temp,route):
        if self.length<len(temp):
            self.length = len(temp)
            self.route = route[:]
        if x<0 or x>=len(matrix) or y<0 or y >= len(matrix[0]):
            return 
        if len(temp) == 0 or matrix[x][y]>temp[-1]:
            temp.append(matrix[x][y])
            route.append([x,y])
            self.dfs(x+1,y,matrix,temp,route)
            self.dfs(x-1,y,matrix,temp,route)
            self.dfs(x,y+1,matrix,temp,route)
            self.dfs(x,y-1,matrix,temp,route)
            temp.pop()
            route.pop()
        return 
```

**记忆化搜索：保存每个节点的搜过的最长的长度。**

```python
class Solution(object):
    def longestIncreasingPath(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: int
        """
        self.direction = [[0,1],[0,-1],[1,0],[-1,0]]
        self.length = 0
        self.route = [[0]*len(matrix[0]) for _ in range(len(matrix)) ]
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                self.length = max(self.length,self.dfs(i,j,matrix))
        return self.length
    
    def dfs(self,x,y,matrix):
        if self.route[x][y] != 0:
            return self.route[x][y]
        for item in self.direction:
            i,j=x+item[0],y+item[1]
            if i>=0 and i<len(matrix) and j>=0 and j<len(matrix[0]) and matrix[i][j] > matrix[x][y]:
                self.route[x][y] = max(self.dfs(i,j,matrix),self.route[x][y])        
        self.route[x][y]+=1
        return self.route[x][y]
```

