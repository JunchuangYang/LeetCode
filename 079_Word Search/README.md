## [79. Word Search](https://leetcode.com/problems/word-search/)

Medium

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example:**

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

题意很简单，在给定的字符矩阵中寻找目标字符串，字符只能用一次。使用DFS重每个字符进行上下左右寻找。

题意思想很简单，深搜很容易就写了出来，可是一直TLE，一直TLE。搜了网上的题解，思想一样，代码差不多，可是还是TLE。

C++：

这是我参考了网上的题解之后第一次AC的代码：

```c++

/*
Runtime: 472 ms
Memory Usage: 211.6 MB
Your runtime beats 8.72 % of cpp submissions.
*/
class Solution {
    
public:
    bool exist(vector<vector<char>>& board, string word) {
        int n = board.size();
        int m = board[0].size();
        int lenw = word.length();
        int flag=0;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if(word[0]==board[i][j]&&dfs(i,j,word,0,board))
                {
                    return true;
                }
            }
        }
        return false;
    }
    bool dfs(int i, int j , string word,int lenw,vector<vector<char>> board)
    {
        if(lenw==word.length()-1)
        {
            return true;
        }
        int n = board.size();
        int m = board[0].size();
        char c = board[i][j];
        board[i][j]='#';
        //printf("%c,%d\n",board[i][j],lenw);        
        if(i-1>=0&&word[lenw+1]==board[i-1][j])
        if(dfs(i-1,j,word,lenw+1,board)) return true;
        
        if(i+1<n&&word[lenw+1]==board[i+1][j])
        if(dfs(i+1,j,word,lenw+1,board)) return true;
            

        
        if(j-1>=0&&word[lenw+1]==board[i][j-1])
        if(dfs(i,j-1,word,lenw+1,board)) return true;
        
        if(j+1<m&&word[lenw+1]==board[i][j+1])
        if(dfs(i,j+1,word,lenw+1,board)) return true;
        
        board[i][j]=c;    
        return false;
    }
};
```

时间472ms，运行效率不高，但不管怎么样不TLE了。

好奇心驱使我看了一下28ms的代码，在这贴出来，

```c++
/*
Runtime: 28 ms
Memory Usage: 9.9 MB
Your runtime beats 80.95 % of cpp submissions.
*/
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        if (word.empty())
            return true;
        if (board.empty() || board[0].empty())
            return false;
        rows = board.size();
        columns = board[0].size();
        int i, j;
        for (i = 0; i < rows; i++) {
            for (j = 0; j < columns; j++)
                if (DFS_helper(board, word, 0, i, j))
                    return true;
        }
        return false;
    }
private:
    int rows, columns;
    
    bool DFS_helper(vector<vector<char>>& board, string& word, 
                    int len, int row, int col) {
        if (row < 0 || col < 0 || row >= rows || col >= columns ||
           word[len] != board[row][col])
            return false;
        if (len == word.size() - 1)
            return true;
        
        char temp = board[row][col];
        board[row][col] = '-';
        bool res = DFS_helper(board, word, len + 1, row + 1, col)
            || DFS_helper(board, word, len + 1, row - 1, col)
            || DFS_helper(board, word, len + 1, row, col + 1)
            || DFS_helper(board, word, len + 1, row, col - 1);
        
        board[row][col] = temp;
        return res;
    }
};
```

我仔细观察，发现与我的没啥不一样啊，为什么时间差这么多，于是我将我的代码按照他的搜索顺序改了一遍，如下：

```c++
/*
Time Limit Exceeded
*/
class Solution {
    
public:
    bool exist(vector<vector<char>>& board, string word) {
        if (word.empty())
            return true;
        if (board.empty() || board[0].empty())
            return false;
        int n = board.size();
        int m = board[0].size();
        int lenw = word.length();
        int flag=0;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if(word[0]==board[i][j]&&dfs(i,j,word,0,board))
                {
                    return true;
                }
            }
        }
        return false;
    }
    bool dfs(int i, int j , string word,int lenw,vector<vector<char>> board)
    {
        int n = board.size();
        int m = board[0].size();

        if(i<0||i>=n||j<0||j>=m||board[i][j]!=word[lenw])
            return false;
 
        if(lenw == word.length()-1) return true;
        char c = board[i][j];
        board[i][j]='#';
        bool res = dfs(i+1,j,word,lenw+1,board)||dfs(i-1,j,word,lenw+1,board)||dfs(i,j+1,word,lenw+1,board)||dfs(i,j-1,word,lenw+1,board);
        board[i][j]=c;
        
        return res;
    }
};
```

如此，代码超时，我就更郁闷了，到底错在什么地方？？？

我自信比较，突然，我发现28ms的代码中在往DFS中传输数组时加了&，而我没有，此时我想，这个加或不加应该影响不会太大吧？？？我还是加上试了一下，如下：

```c++
/*
Runtime: 32 ms
Memory Usage: 9.9 MB
*/
class Solution {
    
public:
    bool exist(vector<vector<char>>& board, string word) {
        if (word.empty())
            return true;
        if (board.empty() || board[0].empty())
            return false;
        int n = board.size();
        int m = board[0].size();
        int lenw = word.length();
        int flag=0;
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if(word[0]==board[i][j]&&dfs(i,j,word,0,board))
                {
                    return true;
                }
            }
        }
        return false;
    }
    bool dfs(int i, int j , string &word,int lenw,vector<vector<char>> &board)
    {
        int n = board.size();
        int m = board[0].size();

        if(i<0||i>=n||j<0||j>=m||board[i][j]!=word[lenw])
            return false;
 
        if(lenw == word.length()-1) return true;
        char c = board[i][j];
        board[i][j]='#';
        bool res = dfs(i+1,j,word,lenw+1,board)||dfs(i-1,j,word,lenw+1,board)||dfs(i,j+1,word,lenw+1,board)||dfs(i,j-1,word,lenw+1,board);
        board[i][j]=c;
        
        return res;
    }
};
```

奇迹出现了，加上之后原本TLE的代码竟然32msAC了。

我自己的理解，在加上&之后，表示用的是exist函数中已经开创好的数组地址，此时可以省掉重新开创数组的时间，而不加&需要再次开创同等大小的数组，占内存并且耗时间。

以下是我的实验：

![1554379593578](https://github.com/JunchuangYang/LeetCode/blob/master/079_Word%20Search/1554379593578.png)

现在懂得了开创数组地址原来可以这么耗费时间。

以上只是我个人的理解，如果有不对的地方欢迎指正。

Python3版：

```python
'''
Runtime: 220 ms, faster than 78.43% of Python3 online submissions for Word Search.
Memory Usage: 14.2 MB, less than 21.74% of Python3 online submissions for Word Search.
'''
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        n = len(board)
        m = len(board[0])
        def dfs(i,j,lenw,board,word):
            if i<0 or i>=n or j<0 or j>=m or word[lenw] != board[i][j]:
                return False
            if lenw == len(word)-1:
                return True;
            c = board[i][j]
            board[i][j]='#'
            res = dfs(i+1,j,lenw+1,board,word) or dfs(i-1,j,lenw+1,board,word) or dfs(i,j+1,lenw+1,board,word) or dfs(i,j-1,lenw+1,board,word)
            board[i][j]=c 
            return res
        for i in range(n):
            for j in range(m):
                if word[0] == board[i][j] and dfs(i,j,0,board,word):
                    return True
        return False
```

