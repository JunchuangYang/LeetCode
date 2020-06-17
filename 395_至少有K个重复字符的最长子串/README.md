# [395. 至少有K个重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-least-k-repeating-characters/)

找到给定字符串（由小写字符组成）中的最长子串 T ， 要求 T 中的每一字符出现次数都不少于 k 。输出 T 的长度。

```
示例 1:
输入:
s = "aaabb", k = 3
输出:
3
最长子串为 "aaa" ，其中 'a' 重复了 3 次。

示例 2:
输入:
s = "ababbc", k = 2
输出:
5
最长子串为 "ababb" ，其中 'a' 重复了 2 次， 'b' 重复了 3 次。
```

****

**递归+分治**

先用hash表统计s中每个字符出现的次数，显然如果字符c出现的次数小于k，c必然不在最长子串里面。根据这个特性可以将原始s分割成多个子串递归地求解问题.


链接：https://leetcode-cn.com/problems/longest-substring-with-at-least-k-repeating-characters/solution/duo-lu-fen-zhi-de-di-gui-fang-fa-zhi-xing-he-nei-c/

```python
class Solution(object):
    def longestSubstring(self, s, k):
        """
        :type s: str
        :type k: int
        :rtype: int
        """
        return self.dfs(s,k)

    def dfs(self,s,k):
        if not s:
            return 0
        dic = self.check(s,k)
        flag = -1
        for i in range(len(s)):
            if dic[s[i]]<k:
                flag = i
        if flag==-1:
            return len(s)
        return max(self.dfs(s[0:flag],k),self.dfs(s[flag+1:],k)) 
    
    def check(self,s,k):
        dic = defaultdict(int)
        for item in s:
            dic[item]+=1
        return dic
```

**修改一下**

```python
class Solution(object):
    def longestSubstring(self, s, k):
        """
        :type s: str
        :type k: int
        :rtype: int
        """
        return self.dfs(s,k)

    def dfs(self,s,k):
        if not s:
            return 0
        dic = self.check(s,k)
        flag = -1
        for i in range(len(s)):
            if dic[s[i]]<k:
                flag = i
        if flag==-1:
            return len(s)
        # 使用split,直接将个数达不到的字符去除分割。
        return max(self.dfs(ss,k) for ss in s.split(s[flag])) 
   
    def check(self,s,k):
        dic = defaultdict(int)
        for item in s:
            dic[item]+=1
        return dic
```

**范例**

```python
class Solution(object):
    def longestSubstring(self, s, k):
        """
        :type s: str
        :type k: int
        :rtype: int
        """
        for c in set(s):
            if s.count(c) < k:
                return max(self.longestSubstring(new,k) for new in s.split(c))
        return len(s)
```

