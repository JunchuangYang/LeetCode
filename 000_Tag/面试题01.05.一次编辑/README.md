# [面试题 01.05. 一次编辑](https://leetcode-cn.com/problems/one-away-lcci/)

字符串有三种编辑操作:插入一个字符、删除一个字符或者替换一个字符。 给定两个字符串，编写一个函数判定它们是否只需要一次(或者零次)编辑。

示例 1:

输入: 
first = "pale"
second = "ple"
输出: True


示例 2:

输入: 
first = "pales"
second = "pal"
输出: False

**字符串编辑：注意分情况**

错了很多次，面向测试用例编程。

```python
class Solution:
    def oneEditAway(self, first: str, second: str) -> bool:
        if first==second:
            return True
        if abs(len(first)-len(second))>1:
            return False
        if len(first)>len(second):
            first,second=second,first
        # 一方为0，一定正确
        if len(first)==0:
            return True
        for i in range(len(first)):
            if first[i]!=second[i]:
                # 相同长度，替换字符
                if len(first)==len(second):
                    first = first[0:i]+second[i]+first[i+1:]
                else: # 不同长度，增加字符
                    first = first[0:i]+second[i]+first[i:]
                break
            if i == len(first)-1:
                first+=second[-1]
        return first==second
```

**看了讨论区的代码，果然简洁。**

```python
class Solution:
    def oneEditAway(self, first: str, second: str) -> bool:
        if first==second:
            return True
        if abs(len(first)-len(second))>1:
            return False
        for i in range(min(len(first),len(second))):
            if first[i]==second[i]:
                pass
            else:
                # 分别对应三种情况
                # first[i+1:]==second[i+1:]：删除（替换）字符
                # first[i+1:]==second[i:] or first[i:] == second[i+1:]：删除字符
                return first[i+1:]==second[i+1:] or first[i+1:]==second[i:] or first[i:] == second[i+1:] 
        return True
```

