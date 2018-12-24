## [Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X` + `II`. The number twenty seven is written as `XXVII`, which is `XX` + `V` + `II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

**Example 1:**

```
Input: "III"
Output: 3
```

**Example 2:**

```
Input: "IV"
Output: 4
```

**Example 3:**

```
Input: "IX"
Output: 9
```

**Example 4:**

```
Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

**Example 5:**

```
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

题意：将给出的罗马数字转换成阿拉伯数字。我的做法是先将用到罗马数字与阿拉伯数字做成map映射，然后将给出的罗马字符串倒着遍历。两个数字相减的情况是当前一个数字小于后一个数字时相减，其余都是根据映射表相加即可。

C++版：

```c++
//104ms，运行时间不理想。
class Solution {
public:
    int romanToInt(string s) {
        map<char,int>m;
        m['I']=1;
        m['V']=5;
        m['X']=10;
        m['L']=50;
        m['C']=100;
        m['D']=500;
        m['M']=1000;
        int len =s.length()-1;
        int sum=0;
        for (int i=len;i>=0;i--)
        {
            if(i>0)
            {
                if(m[s[i]]>m[s[i-1]])
                {
                    sum += (m[s[i]]-m[s[i-1]]);
                    i--;
                }
                else
                    sum+=m[s[i]];
            }
            else
                sum+=m[s[i]];
        }
        return sum;
    }
};
```

Python3版：

```c++
#运行时间：196ms
class Solution:
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        digit ={"I":1,"V":5,"X":10,"L":50,"C":100,"D":500,"M":1000}
        sum1 = 0
        for i in range(len(s)-1,-1,-1):
            if(i>0):
                if(digit[s[i-1]]<digit[s[i]]):
                    sum1+=digit[s[i]]-digit[s[i-1]]*2
                else:
                    sum1+=digit[s[i]]
            else:
                sum1+=digit[s[i]]
        return sum1
```

评论区看到的代码：

```python
# 运行时间116ms,超过了99.96%的Python代码
class Solution:
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        r = {'I':1,'V':5,'X':10,'L':50,'C':100,'D':500,'M':1000}
        n = 0
        j = 'M'
        for i in s:
            n += r[i]
            if r[j] < r[i]:#如果前一个数字小于后一个数字，相减。
                n -= 2*r[j]
            j = i
        return n
```

