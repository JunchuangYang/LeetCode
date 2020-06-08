# [49. 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

示例:

输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
说明：

所有输入均为小写字母。
不考虑答案输出的顺序。

```python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        dic = {}
        for i in range(len(strs)):
            s = list(strs[i])
            s.sort() # 排序
            s1 = ''.join(s)
            # 放入字典
            if dic.get(s1,0)==0:
                dic[s1]=[strs[i]]
            else:
                dic[s1].append(strs[i])
        res = []
        for item in dic.keys():
            res.append(dic[item])
        return res
```

