# [36. 有效的数独](https://leetcode-cn.com/problems/valid-sudoku/)

判断一个 9x9 的数独是否有效。只需要根据以下规则，验证已经填入的数字是否有效即可。

数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。

**思路一：模拟**

```python
class Solution(object):
    def isValidSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: bool
        """
        column = [[] for i in range(9)] # 列
        block = [[] for i in range(9)] # 块
        for i in range(len(board)):
            for j in range(len(board)):
                column[j].append(board[i][j])
                block[i//3*3+j//3].append(board[i][j])
        
        if self.check(board) and self.check(column) and self.check(block):
            return True
        return False

    def check(self,lists):
        n,m = len(lists),len(lists[0])
        for i in range(n):
            for j in range(m):
                if lists[i][j]!='.' and lists[i].count(lists[i][j])>1:
                    return 0
        return 1

```

**思路二：[一次迭代](<https://leetcode-cn.com/problems/valid-sudoku/solution/you-xiao-de-shu-du-by-leetcode/>)**

方法：一次迭代
首先，让我们来讨论下面两个问题：

- 如何枚举子数独？
  	- 可以使用 box_index = (row / 3) * 3 + columns / 3，其中 / 是整数除法。
- 如何确保行 / 列 / 子数独中没有重复项？
  - 可以利用 value -> count 哈希映射来跟踪所有已经遇到的值。
- 现在，我们完成了这个算法的所有准备工作：

- 遍历数独。
- 检查看到每个单元格值是否已经在当前的行 / 列 / 子数独中出现过：
   - 如果出现重复，返回 false。
   - 如果没有，则保留此值以进行进一步跟踪。
- 返回True

```python
class Solution(object):
    def isValidSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: bool
        """
        row = [{} for i in range(9)] # 
        column = [{} for i in range(9)] # 列
        block = [{} for i in range(9)] # 块
        for i in range(len(board)):
            for j in range(len(board)):
                if board[i][j] != '.':
                    num = int(board[i][j])
                    block_index = i//3*3+j//3
                    row[i][num] = row[i].get(num,0)+1
                    column[j][num] = column[j].get(num,0)+1
                    block[block_index][num] = block[block_index].get(num,0)+1
                    if row[i][num] > 1 or column[j][num] > 1 or block[block_index][num] >1:
                        return False
        return True

```

**两种方法时间倒是一样多。**