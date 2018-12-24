## [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

**Easy**

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

```
Input: ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

**Note:**

All given inputs are in lowercase letters `a-z`.

题意：给出所给字符串数组中所有字符串的最长公共子串。取第一个字符串，一个字符一个字符的遍历所有的字符串，如果当现在的遍历长度大于匹配的字符串，或匹配字符不相等时返回公共字符串。

C++版：

```c++
//Runtime：8 ms, faster than 37.50% of C++ online submissions for Longest Common Prefix. 
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string str="";
        int len = strs.size()==0?0:strs[0].size();
        for(int i=0;i<len;i++)
        {
            char c = strs[0][i];
            for (int j=1;j<strs.size();j++)
            {
                if(i>=strs[j].size()||strs[j][i]!=c)
                    return str;
            }
            str+=c;
        }
        return str;
    }
};
```

**在LeetCode做题的过程中，遇到"reference binding to null pointer of type ‘value_type’" 这个问题**，发现是Vector越界的问题，需要在程序开始是判断所给vector是否为空。

Python3版：

```python
#Runtime: 60 ms, faster than 25.39% of Python3 online submissions for Longest Common Prefix.
class Solution:
    def longestCommonPrefix(self, strs):
        """
        :type strs: List[str]
        :rtype: str
        """
        if len(strs)==0:
            return ""
        str1=""
        for i in range(len(strs[0])):
            c = strs[0][i]
            for j in range(1,len(strs)):
                if i>=len(strs[j]) or c!=strs[j][i]:
                    return str1
            str1+=c
        return str1
```

