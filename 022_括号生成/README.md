# [22. 括号生成](https://leetcode-cn.com/problems/generate-parentheses/)

数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

示例：

```
输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]
```

**递归：先放'('，放的时候判断'('的个数是否大于')'的个数。**

```python
class Solution(object):
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        x = "("*n
        y = ")"*n
        res = []
        self.dfs(x,y,res,[])
        return ["".join(item) for item in res]
    def dfs(self,x,y,res,temp):
        #print(x,y)
        if not x and not y:
            res.append(temp[:])
            return 
        if x and temp.count('(') >=temp.count(')'):
            temp.append('(')
            self.dfs(x[1:],y,res,temp)
            temp.pop()
        if y and temp.count('(') >=temp.count(')'):
            temp.append(')')
            self.dfs(x,y[1:],res,temp)
            temp.pop()
        return
```

