# [139. 单词拆分](https://leetcode-cn.com/problems/word-break/)

给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，判定 s 是否可以被空格拆分为一个或多个在字典中出现的单词。

说明：

拆分时可以重复使用字典中的单词。
你可以假设字典中没有重复的单词。

```python
示例 1：

输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。
示例 2：

输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
     注意你可以重复使用字典中的单词。
示例 3：

输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```

**普通递归超时，加了一个判别数组当前字符串是否检查过，勉强AC**

```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """
        self.dp = {} # 判别数组
        if s in wordDict:
            return True
        if self.dfs(s,wordDict):
            return True
        return False
    def dfs(self,s,wordDict):
        if not s:
            return True
        if s in self.dp.keys():
            return self.dp[s]
        for i in range(len(s)+1):
            if s[0:i] in wordDict:
                if self.dfs(s[i:],wordDict):
                    return True
            else:
                self.dp[s[0:i]]=False
        return False
```

**动态规划：**

解题思路
看到本题，我的第一个想法就是通过wordDict中单词的组合去与s进行匹配，如果能匹配上，则证明是可以拆分的，这是一种暴力的解法，背后是一种“回溯法”的解题思路。这种方法所能求出所有的结果，但是由于时间复杂度比较高，所以不能够通过。

那我们就要另寻其他的方法。刚才的方法其实是通过组合以后整体的去比较“s”与“组合的串”之间是不是匹配。其实我们可以通过每个word与“s”是不是匹配。我们定义一个布尔dp数组，长度为s.length + 1。dp[i]表示字符串s的前i个字符能否拆分成wordDict。我们将每次的都记录下来。
链接：https://leetcode-cn.com/problems/word-break/solution/139-dan-ci-chai-fen-by-ming-zhi-shan-you-m9rfkvkda/

```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """
        if s in wordDict:
            return True
        index = 0
        dp = [False for i in range(len(s)+1)]
        dp[0] = True
        for i in range(1,len(s)+1):
            for j in range(i):
                if dp[j] and s[j:i] in wordDict:
                    dp[i] = True
        return dp[-1]
```

