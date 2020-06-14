# [212. 单词搜索 II](https://leetcode-cn.com/problems/word-search-ii/)

给定一个二维网格 board 和一个字典中的单词列表 words，找出所有同时在二维网格和字典中出现的单词。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

```
示例:

输入: 
words = ["oath","pea","eat","rain"] and board =
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]
```

说明:
你可以假设所有输入都由小写字母 a-z 组成。

提示:

你需要优化回溯算法以通过更大数据量的测试。你能否早点停止回溯？
如果当前单词不存在于所有单词的前缀中，则可以立即停止回溯。什么样的数据结构可以有效地执行这样的操作？散列表是否可行？为什么？ 前缀树如何？如果你想学习如何实现一个基本的前缀树，请先查看这个问题： 实现Trie（前缀树）。



**递归：超时**

```python
class Solution(object):
    def findWords(self, board, words):
        """
        :type board: List[List[str]]
        :type words: List[str]
        :rtype: List[str]
        """
        res = []
        flag = 0
        words.sort()
        for item in words:
            flag = 0
            for i in range(len(board)):
                for j in range(len(board[0])):
                    if board[i][j] == item[0]:
                        if self.dfs(i,j,board,item,0,[]):
                            res.append(item)
                            flag = 1
                            break
                if flag:
                    break
        return res
    def dfs(self,i,j,board,item,k,temp):
        if len(item) == k:
            return True
        if i<0 or i>=len(board) or j <0 or j>=len(board[0]):
            return False
        if board[i][j] == item[k]:
            board[i][j] ='#'
            flag = self.dfs(i+1,j,board,item,k+1,temp) or \
            self.dfs(i-1,j,board,item,k+1,temp) or \
            self.dfs(i,j+1,board,item,k+1,temp) or \
            self.dfs(i,j-1,board,item,k+1,temp) 
            board[i][j] = item[k]
            if flag:
                return True
        return False
```

