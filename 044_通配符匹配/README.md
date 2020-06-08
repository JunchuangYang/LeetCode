# [44. 通配符匹配](https://leetcode-cn.com/problems/wildcard-matching/)

给定一个字符串 (s) 和一个字符模式 (p) ，实现一个支持 '?' 和 '*' 的通配符匹配。

'?' 可以匹配任何单个字符。
'*' 可以匹配任意字符串（包括空字符串）。
两个字符串完全匹配才算匹配成功。

```
说明:

s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 ? 和 *。
示例 1:

输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
示例 2:

输入:
s = "aa"
p = "*"
输出: true
解释: '*' 可以匹配任意字符串。
示例 3:

输入:
s = "cb"
p = "?a"
输出: false
解释: '?' 可以匹配 'c', 但第二个 'a' 无法匹配 'b'。
示例 4:

输入:
s = "adceb"
p = "*a*b"
输出: true
解释: 第一个 '*' 可以匹配空字符串, 第二个 '*' 可以匹配字符串 "dce".
示例 5:

输入:
s = "acdcb"
p = "a*c?b"
输出: false

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/wildcard-matching
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

**深搜：超时。这个题和以前做的不一样的地方在于*可以无限匹配。**

```python
class Solution(object):
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        if not s:
            for i in p:
                if i != '*':
                    return False
            return True
        p = list(p)
        # 清理输入数据，不管 a****bc**cc 中有多少个星号，它们都可以简化为 a*bc*cc。
        # 这样的清理有助于减少递归深度。
        for i in range(len(p)-1,0,-1):
            if p[i]==p[i-1] and p[i]=='*':
                p.pop(i)
        return self.dfs(s,''.join(p))
    def dfs(self,s,p):
        if not s  and not p:
            return True
        if len(p)>0 and p[0]=='*':
            if s:
                return self.dfs(s,p[1:]) or self.dfs(s[1:],p) or self.dfs(s[1:],p[1:])
            else:
                return self.dfs(s,p[1:])
        if len(p)>0 and len(s)>0 and (p[0]==s[0] or p[0]=='?'):
            return self.dfs(s[1:],p[1:])
        return False
```

**优化深搜：加了个字典保存当前处理过的字符串，但还是超时，剩2组数据没有过。**

```python
class Solution(object):
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        self.dp = {}
        p = list(p)
        for i in range(len(p)-1,0,-1):
            if p[i]==p[i-1] and p[i]=='*':
                p.pop(i)
        return self.dfs(s,''.join(p))
    def dfs(self,s,p):
        dp = self.dp
        if (s,p) in dp:
            return dp[(s,p)]
        if p == s or p == '*':
            dp[(s,p)] = True
        elif s=='' or p == '':
            dp[(s,p)] = False
        elif p[0]=='*' :
            dp[(s,p)] =  self.dfs(s,p[1:]) or self.dfs(s[1:],p) 
        elif p[0]==s[0] or p[0]=='?':
            dp[(s,p)] = self.dfs(s[1:],p[1:])
        else:
            dp[(s,p)] = False
        return dp[(s,p)]
```

**[回溯法](<https://leetcode-cn.com/problems/wildcard-matching/solution/tong-pei-fu-pi-pei-hui-su-fa-dong-tai-gui-hua-lian/>)**

当第一次出现星号时,它可能对应从0开始的多个元素，所以我从对应0个元素开始去假设（即si不变，pi+1，而且此时用si_match,pstar去记录当前的si与pi的位置），一点点去试，如果当前探索元素不对应，则回到最开始出现星号的位置，再假设对应1个元素（即si_match += 1 ，si = si_match），一点点去试，直至出现第二个星号（此时对前一个星号的假设完全成立，更新此时的si_match,pstar）或者在这种假设下一直成功探索到s的最后一个元素为止。
**循环结束时s探索完毕，但p不一定探索完毕，当此时s中没有元素再能与p剩下的元素相对应，除非p剩下的全是星号，否则不能匹配。**

```python
class Solution(object):
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        si = pi = 0
        lens , lenp = len(s),len(p)
        pstar = -1  # 记录最近的*出现的位置
        si_match = 0 # 表示si_match 前的字符串完全匹配
        while si < lens:
            if pi<lenp and (s[si] == p[pi] or p[pi] == '?'): # 当前匹配
                si += 1
                pi += 1
            elif pi<lenp and p[pi] == '*':
                pstar = pi
                si_match = si
                pi += 1 # p往后走,假设匹配0个
            # 前面全部匹配，p[pi] != s[si],回溯找*号的位置
            elif pstar != -1:
                pi = pstar + 1 # 回溯*号最开始的地方
                si_match += 1 # 回溯到匹配*号0个位置的地方，假设匹配1,2,3...个，依次递增，直至第二个*出现
                si = si_match
            else:
                return False
        # s匹配结束，p剩下的必须全是*才算匹配成功
        return  list(p[pi:]).count('*') == len(p[pi:])
        
```

