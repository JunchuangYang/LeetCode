### Longest Substring Without Repeating Characters

Given a string, find the length of the longest substring without repeating characters.  

Example 1:  

Input: "abcabcbb"  
Output: 3   
Explanation: The answer is "abc", with the length of 3.   
Example 2:  

Input: "bbbbb"  
Output: 1  
Explanation: The answer is "b", with the length of 1.  
Example 3:

Input: "pwwkew"  
Output: 3  
Explanation: The answer is "wke", with the length of 3.  
Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

题意：给你一个字符串，找出给出的字符串中字符不重复的最大子串。注意：必须是最大子字符串

思路：遍历一次给出的字符串，在遍历过程中，用map<char,int>记录出现的字符,key=字符，value=index（字符在字符串中的索引位置），同时记录开始遍历的起始位置en，记录遍历长度sum。当出现重复字符串的时候，判断max是否更新，更新起始位置en（出现此字符的位置+1），更新长度sum=i-en+1，更新map中此字符的值。

其中我还遇到了一个问题，当用map.find()查找字符a（假设）是否已经存在时，会出现这样一个情况：起始位置en>字符a在map中的值，也就是字符a在上一个不重复的子串中出现过。所以当出现这样一个情况时，我判定为字符a在新一轮统计的子串中没有出现过，更新map['a']的值。

可能语言表达的不清楚，参见代码。

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        map<char,int>str;   
        int len = s.size();
        int max = 0,beg=0,en=0,sum=0;
        for (int i=0;i<len;i++)
        {
            if(str.find(s[i])==str.end()||en>str[s[i]])
            {
                str[s[i]]=i;
                sum++;
            }
            else
            {
                if(max<sum)
                {
                    max=sum;
                }
                en=str[s[i]]+1;
                sum=i-en+1;
                str[s[i]]=i;
            }
        }
        return max<sum?sum:max;
    }
};
```

我在网上找到一个题解，思路和我的差不多，可是人家表述的很清楚。（Orz）

参考：https://blog.csdn.net/wl_ss/article/details/79111332

思路：用一个map记录各个字符的下标，首先将所有的下表初始化为-1，然后向后遍历的过程中记录当前字符的下标，并将当前字符的下标与上一个相同字符的下标做差值，即可得到它们之间无重复字符的子串。然后用Max记录更新最大子串数，并更新map存储的下标值，即从这个重复值重新开始数字符串的数（最大无重复字符子串必定在两个重复字符之间，或者是整个字符串的长度）



这种方法好像被叫做滑动窗口法


"滑动窗口" 
​    比方说 abcabccc 当你右边扫描到abca的时候你得把第一个a删掉得到bca，
​    然后"窗口"继续向右滑动，每当加到一个新char的时候，左边检查有无重复的char，
​    然后如果没有重复的就正常添加，
​    有重复的话就左边扔掉一部分（从最左到重复char这段扔掉），在这个过程中记录最大窗口长度



其实和上面的思路是一样的！


```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        map<char,int> book;
        int i,Max=0,pre=-1;
        for(i=0;i<s.length();i++) book[s[i]]=-1;
        for(i=0;i<s.length();i++)
        {
            pre=max(pre,book[s[i]]);//更新map中各个字符的下标
            Max=max(Max,i-pre); //保存暂时的最大无重复子串长度
            book[s[i]]=i; //计算差值后继续更新
        }
        return Max;
    }
};

```
两种方法运行时间的对比，其实都差不多：

![](https://i.imgur.com/SitwQz8.png)



