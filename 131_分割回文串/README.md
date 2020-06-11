# [131. 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)

给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。

返回 s 所有可能的分割方案。

示例:

输入: "aab"
输出:
[
  ["aa","b"],
  ["a","a","b"]
]

**递归**

```python
class Solution(object):
    def partition(self, s):
        """
        :type s: str
        :rtype: List[List[str]]
        """
        if not s:
            return None
        
        self.res = []
        self.dp = {} # 标记数组，看看当前串是否出现过。
        self.dfs(s,[])
        return self.res

    def dfs(self,s,temp):
        if not s:
            if tuple(temp) in self.dp.keys():
                return 
            self.dp[tuple(temp)] = 1
            self.res.append(temp[:])
        for i in range(1,len(s)+1):
            if s[:i] == s[:i][::-1]:# 递归前首先判断是否是回文串，否则会超时
                temp.append(s[:i])
                self.dfs(s[i:],temp)
                temp.pop()
        return 

```

<https://leetcode-cn.com/problems/palindrome-partitioning/solution/python3-di-gui-jian-dan-yi-dong-by-ting-ting-28/>

循环1……len(s)，分割s，如果分割的前段是回文串，用前段加partition(后段)的每个值。

```python
class Solution(object):
    def partition(self, s):
        """
        :type s: str
        :rtype: List[List[str]]
        """
        if not s:
            return [[]]
        if len(s) == 1:
            return [[s]]
        ret = []
        for i in range(1, len(s)+1):a
            if s[:i][::-1] == s[:i]:
                ret += [[s[:i]]+item for item in self.partition(s[i:])]
        return ret
```

