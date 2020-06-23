# [1419. 数青蛙](https://leetcode-cn.com/problems/minimum-number-of-frogs-croaking/)

给你一个字符串 croakOfFrogs，它表示不同青蛙发出的蛙鸣声（字符串 "croak" ）的组合。由于同一时间可以有多只青蛙呱呱作响，所以 croakOfFrogs 中会混合多个 “croak” 。请你返回模拟字符串中所有蛙鸣所需不同青蛙的最少数目。

注意：要想发出蛙鸣 "croak"，青蛙必须 依序 输出 ‘c’, ’r’, ’o’, ’a’, ’k’ 这 5 个字母。如果没有输出全部五个字母，那么它就不会发出声音。

如果字符串 croakOfFrogs 不是由若干有效的 "croak" 字符混合而成，请返回 -1 。

示例 1：

输入：croakOfFrogs = "croakcroak"
输出：1 
解释：一只青蛙 “呱呱” 两次
示例 2：

输入：croakOfFrogs = "crcoakroak"
输出：2 
解释：最少需要两只青蛙，“呱呱” 声用黑体标注
第一只青蛙 "crcoakroak"
第二只青蛙 "crcoakroak"
示例 3：

输入：croakOfFrogs = "croakcrook"
输出：-1
解释：给出的字符串不是 "croak" 的有效组合。
示例 4：

输入：croakOfFrogs = "croakcroa"
输出：-1


提示：

1 <= croakOfFrogs.length <= 10^5
字符串中的字符只有 'c', 'r', 'o', 'a' 或者 'k'

**深搜超时：确实是我想的太复杂了。**

```python
class Solution:
    def minNumberOfFrogs(self, croakOfFrogs: str) -> int:
        self.flag = [0 for _ in range(len(croakOfFrogs))]
        s = 'croak'
        self.ans=[]
        kk = []
        for i in range(len(croakOfFrogs)):
            if croakOfFrogs[i] == 'c':
                kk.append(i)
                end = self.dfs(croakOfFrogs,s,i,0)
                if end:
                    self.ans.append([i,end])
        if len(self.ans)*5 == len(croakOfFrogs):
            nums = 1
            for i in range(0,len(self.ans)):
                n = len(list(filter(lambda x:x>self.ans[i][0] and x<self.ans[i][1],kk)))
                if n>=nums:
                    nums=n+1
            return nums
        return -1
    
    def dfs(self,croakOfFrogs,s,start,index):
        if index == len(s):
            return start-1
        for i in range(start,len(croakOfFrogs)):
            if croakOfFrogs[i] == s[index] and self.flag[i]==0:
                self.flag[i] = 1
                index+=1
                ans = self.dfs(croakOfFrogs,s,i+1,index)
                if ans:
                    return ans
                index-=1
                self.flag[i]=0
        return False
```

**题解：[判断当前有多少个字母c](<https://leetcode-cn.com/problems/minimum-number-of-frogs-croaking/solution/pan-duan-dang-qian-you-duo-shao-ge-zi-mu-c-by-simo/>)**

```python
class Solution:
    def minNumberOfFrogs(self, croakOfFrogs: str) -> int:
        char_c = char_r = char_o = char_a = char_k = 0
        res = 0
        flag = 0
        for let in croakOfFrogs:
            if let == 'c': char_c+=1
            elif let == 'r': char_r+=1
            elif let == 'o': char_o+=1
            elif let == 'a': char_a+=1
            elif let == 'k': 
                char_k+=1
                res = max(res,char_c)
                if char_c>=char_r and char_r>=char_o and char_o>=char_a and char_a>=char_k:
                    char_c-=1
                    char_r-=1
                    char_o-=1
                    char_a-=1
                    char_k-=1
                else:
                    flag = 1
                    break
            if char_c>=char_r and char_r>=char_o and char_o>=char_a and char_a>=char_k:
                pass
            else:
                flag = 1
                break
        if flag or char_c or char_r or char_o or char_a or char_k:
            return -1 
        return res
```

