## [12.Integer to Roman](https://leetcode.com/problems/integer-to-roman/)

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

Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.

**Example 1:**

```
Input: 3
Output: "III"
```

**Example 2:**

```
Input: 4
Output: "IV"
```

**Example 3:**

```
Input: 9
Output: "IX"
```

**Example 4:**

```
Input: 58
Output: "LVIII"
Explanation: L = 50, V = 5, III = 3.
```

**Example 5:**

```
Input: 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

题目大意：将给出的数字转换成罗马数字，题意简单，不难理解。需要注意在数位上的数字是4和9的情况是特例，还有就是超过5的数字的表示方法。

C++版：

```c++
class Solution {
public:
    string intToRoman(int num) {
        string s;
        int str[5]={0};
        for (int i = 0; i<4;i++)
        {
            str[i]=num%10;
            num/=10;
        }
        for(int i=3;i>=0;i--)
        {
            if(str[i]!=0)
            {
                if(str[i]==4||str[i]==9)
                {
                    switch(i){
                    case 0:s+=(str[i]==4?"IV":"IX");break;
                    case 1:s+=(str[i]==4?"XL":"XC");break;
                    case 2:s+=(str[i]==4?"CD":"CM");break;
                    }
                }
                else
                {
                    int num =str[i];
                    string c1,c2;
                    switch(i){
                    case 0:c1="V",c2="I";break;
                    case 1:c1="L",c2="X";break;
                    case 2:c1="D",c2="C";break;
                    case 3:c1="X",c2="M";break;
                    }
                    if(str[i]>=5)
                        s+=c1,num-=5;
                    while(num--) s+=c2;
                }
            }
        }
        return s;
    }
};
```

Python3版：

对Python代码不太熟悉，也是用模拟的方法写的，没有发挥Python代码的优势。

```python
class Solution:
    def intToRoman(self, num):
        """
        :type num: int
        :rtype: str
        """
        s = str(num)
        k = ""
        n = len(s) -1
        for i in s[::1]:
            if n==0:
                c1 = "IV"
                c2 = "IX"
                c3 = "V"
                c4 = "I"
            elif n==1:
                c1 = "XL"
                c2 = "XC"
                c3 = "L"
                c4 = "X"
            elif n==2:
                c1 = "CD"
                c2 = "CM"
                c3 = "D"
                c4 = "C"
            elif n==3:
                c1 = ""
                c2 = ""
                c3 = ""
                c4 = "M"
            o = int(i)
            if o==4:
                k+=c1
            elif o==9:
                k+=c2
            else:
                if (o>5) or (o==5):
                    k+=c3
                    o-=5
                while o>0:
                    k+=c4
                    o-=1
            n-=1
        return k
```

在评论去看到的代码，贴出来供学习：

```python
class Solution:
    #clear
    def intToRoman(self, num):
        dict = ["M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"]
        nums = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1]
        result = ""
        for letter, n in zip(dict, nums):
            result += letter * int(num / n)
            num %= n
        return result
```

