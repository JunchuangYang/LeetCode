# [140. 单词拆分 II](https://leetcode-cn.com/problems/word-break-ii/)

给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，在字符串中增加空格来构建一个句子，使得句子中所有的单词都在词典中。返回所有这些可能的句子。

说明：

分隔时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。

```
示例 1：

输入:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
输出:
[
  "cats and dog",
  "cat sand dog"
]
示例 2：

输入:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
输出:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
解释: 注意你可以重复使用字典中的单词。
示例 3：

输入:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
输出:
[]
```

**递归+保存状态**

```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: List[str]
        """
        self.dp = {} # 保存搜过的字符串的解
        return self.dfs(s,wordDict)
    def dfs(self,s,wordDict):
        if not s:
            return [''] # 有解
        if s in self.dp.keys():
            return self.dp[s] # 有解
        res = []
        for i in range(1,len(s)+1):
            if s[0:i] in wordDict:
                temp = self.dfs(s[i:],wordDict)
                for item in temp:
                    if item == '':
                        res.append(s[0:i])
                    else:
                        res.append(s[0:i]+' ' +str(item))
        self.dp[s] = res
        return  res
```

