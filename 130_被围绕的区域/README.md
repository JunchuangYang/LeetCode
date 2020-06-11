# [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

给定一个二维的矩阵，包含 `'X'` 和 `'O'`（**字母 O**）。

找到所有被 `'X'` 围绕的区域，并将这些区域里所有的 `'O'` 用 `'X'` 填充。

```
示例:

X X X X
X O O X
X X O X
X O X X

运行你的函数后，矩阵变为：

X X X X
X X X X
X X X X
X O X X
```

解释:

被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。

```python
class Solution(object):
    def solve(self, board):
        """
        :type board: List[List[str]]
        :rtype: None Do not return anything, modify board in-place instead.
        """

        for i in range(1,len(board)-1):
            for j in range(1,len(board[0])-1):
                temp = []
                if board[i][j] == 'O':
                    if not self.dfs(i,j,board,temp):
                        for item in temp:
                            board[item[0]][item[1]] = 'O'
        
    
    def dfs(self,i,j,board,temp):
        if i<0 or i>=len(board) or j<0 or j>=len(board[0]):
            return False 
        if board[i][j]=='X':
            return True
        
        if board[i][j]=='O':
            temp.append([i,j])
            board[i][j]='X'
            return self.dfs(i-1,j,board,temp) and self.dfs(i,j+1,board,temp) and self.dfs(i+1,j,board,temp) and self.dfs(i,j-1,board,temp)
```

