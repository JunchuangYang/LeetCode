# [97. 交错字符串](https://leetcode-cn.com/problems/interleaving-string/)

给定三个字符串 s1, s2, s3, 验证 s3 是否是由 s1 和 s2 交错组成的。

示例 1:

输入: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
输出: true
示例 2:

输入: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
输出: false

**动态规划**

```python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        if not s1:
            return s2==s3
        if not s2:
            return s1==s3
        if len(s1)+len(s2)!=len(s3):
            return False
        # dp[i][j]表示从字符串s1的第i个字符到s2的第j个字符是匹配的
        dp = [[0]*(len(s2)+1) for _ in range(len(s1)+1)]
        for i in range(1,len(s1)+1):
            if s1[i-1]==s3[i-1]:
                dp[i][0] = 1
            else:
                break
        for i in range(1,len(s2)+1):
            if s2[i-1]==s3[i-1]:
                dp[0][i] = 1
            else:
                break
        for i in range(1,len(s1)+1):
            for j in range(1,len(s2)+1):
                if  s2[j-1] == s3[i+j-1]:
                    dp[i][j] = dp[i][j-1]
                if  dp[i][j]==0 and s1[i-1] == s3[i+j-1]:
                    dp[i][j] = dp[i-1][j]
        return dp[len(s1)][len(s2)] == 1
```

**简化后的，不过时间复杂度没有变化**

```python
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        if not s1:
            return s2==s3
        if not s2:
            return s1==s3
        if len(s1)+len(s2)!=len(s3):
            return False
        # dp[i][j]表示从字符串s1的第i个字符到s2的第j个字符是匹配的
        dp = [[False]*(len(s2)+1) for _ in range(len(s1)+1)]
        dp[0][0]= True
        for i in range(0,len(s1)+1):
            for j in range(0,len(s2)+1):
                if  j>0:
                    dp[i][j] |= (dp[i][j-1] and s2[j-1]==s3[i+j-1])
                if  i>0 :
                    dp[i][j] |= (dp[i-1][j] and s1[i-1]==s3[i+j-1])
        return dp[len(s1)][len(s2)] 
```

