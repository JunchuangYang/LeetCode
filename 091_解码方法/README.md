# [91. 解码方法](https://leetcode-cn.com/problems/decode-ways/)

一条包含字母 A-Z 的消息通过以下方式进行了编码：

'A' -> 1
'B' -> 2
...
'Z' -> 26
给定一个只包含数字的非空字符串，请计算解码方法的总数。

示例 1:

输入: "12"
输出: 2
解释: 它可以解码为 "AB"（1 2）或者 "L"（12）。
示例 2:

输入: "226"
输出: 3
解释: 它可以解码为 "BZ" (2 26), "VF" (22 6), 或者 "BBF" (2 2 6) 。

**思路：递归求出字符串所有的排列方法，并判断是否符合。超时。**

```python
class Solution(object):
    def numDecodings(self, s):
        """
        :type s: str
        :rtype: int
        """
        self.dp = {} # 标记数组，检查当前字母组合是否出现过
        self.dfs(s,[])
        return len(self.dp)
    def dfs(self,s,temp):
        if not s:
            for item in temp:
                if item[0]=='0' or int(item)<=0 or int(item)>=27:
                    return
            #print(temp)
            self.dp[tuple(temp)] = self.dp.get(tuple(temp),0) + 1
            return
        for i in range(len(s)):
            temp.append(s[0:i+1])
            self.dfs(s[i+1:],temp)
            temp.pop()
        return
```

**动态规划还是不行，想了很长时间也看了题解还是不太懂。**

<https://leetcode-cn.com/problems/decode-ways/solution/java-jian-dan-dp-shi-jian-ji-bai-100-by-jiafeilee/>

```python
class Solution(object):
    def numDecodings(self, s):
        """
        :type s: str
        :rtype: int
        """
        if s[0]=='0':
            return 0
        dp = [0 for i in range(len(s)+1)]
        dp[0] = dp[1] = 1
        for i in range(2,len(s)+1):
            num = 0
            if s[i-1]=='0':
                dp[i] = 0
            else:
                dp[i] += dp[i-1]
            if 27>int(s[i-2])*10+int(s[i-1])>9:
                dp[i] += dp[i-2]

        return dp[len(s)]

```

