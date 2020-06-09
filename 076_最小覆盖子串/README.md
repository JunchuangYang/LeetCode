# [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)

给你一个字符串 S、一个字符串 T，请在字符串 S 里面找出：包含 T 所有字符的最小子串。

示例：

输入: S = "ADOBECODEBANC", T = "ABC"
输出: "BANC"
说明：

如果 S 中不存这样的子串，则返回空字符串 ""。
如果 S 中存在这样的子串，我们保证它是唯一的答案。

**滑动窗口：<https://leetcode-cn.com/problems/minimum-window-substring/solution/zui-xiao-fu-gai-zi-chuan-by-leetcode-solution/>**

以下代码超时：

```python
class Solution(object):
    def minWindow(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: str
        """
        left,right = 0,0
        dic = {}
        res = ''
        for i in range(len(t)):
            dic[t[i]] = dic.get(t[i],0)+1
        d = {}
        for r in range(len(s)+1):
            if self.check(s[left:r],dic):
                while self.check(s[left:r],dic):
                    #print(s[left:r])
                    if not res:
                        res = s[left:r]
                    elif len(res) > r-left: 
                        res = s[left:r]
                    left+= self.find_l(s[left:r],dic)
        return res

    def find_l(self,s,dic):
        l = -1
        for key in dic.keys():
            l = min(l,s.find(key))
        return 1 if l == -1 or l==0  else l

    def check(self, s,dic):
        for key in dic.keys():
            if s.count(key)<dic[key]:
                return False
        return True
```

**优化后的版本:[题解](<https://leetcode-cn.com/problems/minimum-window-substring/solution/hua-dong-chuang-kou-suan-fa-tong-yong-si-xiang-by-/>)**

```python
class Solution(object):
    def minWindow(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: str
        """
        left,right = 0,0
        dic = {}
        res = ''
        for i in range(len(t)): # 对目标字符串中的字符计数，防止有重复字符
            dic[t[i]] = dic.get(t[i],0)+1
        window = {} # 滑动窗口
        valid = 0 # 标记字符，当标记个数等于dic中的个数，说明当前window包含了所有目标字符
        while right<len(s):
            window[s[right]] = window.get(s[right],0)+1
            if window[s[right]] == dic.get(s[right],-1):
                valid+=1
            while valid == len(dic):
                if not res:
                    res = s[left:right+1]
                elif len(res) > right-left+1:
                    res = s[left:right+1]
                # window减小，left++
                window[s[left]]-=1
                if dic.get(s[left],-1) != -1: # 不是目标字符串中的值
                    # 是目标字符串中的值，但是减去后个数比目标字符串少
                    if dic.get(s[left],-1) > window[s[left]]:
                        valid-=1
                left+=1
            right+=1
        return res
```

