### String to Integer (atoi)

Implement `atoi` which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

**Note:**

- Only the space character `' '` is considered as whitespace character.
- Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31,  2^31 − 1]. If the numerical value is out of the range of representable values, **INT_MAX** (2^31 − 1) or **INT_MIN** (−2^31) is returned.

**Example 1:**

```
Input: "42"
Output: 42
```

**Example 2:**

```
Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.
```

**Example 3:**

```
Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```

**Example 4:**

```
Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.
```

**Example 5:**

```
Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−2^31) is returned.
```

题意：将给出的字符传中包含的数字字符转换为int型数字。需要注意的基本都在给出的样例中了。

C++版：

```c++
class Solution {
public:
    int myAtoi(string str) {
        int flag=0;
        int f=0,sign=0;
        long long sum=0;
        for(int i=0;i<str.length();i++)
        {
            if(flag==0&&str[i]==' ')
            {
                continue;
            }
            if(flag==0&&(str[i]>'9'||str[i]<'0')&&str[i]!='-'&&str[i]!='+')
            {
                return 0;
            }
            if(flag==0&&str[i]=='-')
            {
                sign=1;
                flag=1;
                continue;
            }
            if(flag==0&&str[i]=='+')
            {
                flag=1;
                continue;
            }
            flag=1;
            if(str[i]>='0'&&str[i]<='9')
            {
                sum = sum*10 + (str[i]-'0');
                
            }
            else
                break;
            if(sum>INT_MAX)
                break;
        }
        
        if(sign==1)
        {
            sum=-sum;
            sum = sum<INT_MIN?INT_MIN:sum;
        }
        else
        {
            sum = sum>INT_MAX?INT_MAX:sum;
        }

        return sum;
    }
};
```

这里我在强调一点，在循环过程中就可以去比较数字是否超过整型32位，如果放到最后一起比较的话可能数字会超过long long 的最大范围，导致出错。

Python3版：使用正则表达式匹配。

```python
class Solution:
    def myAtoi(self, str):
        """
        :type str: str
        :rtype: int
        """
        try:
            num = re.search('(^[\+\-]?\d+)',str.strip()).group()
            num = int(num)
            if(num>2147483647):
                num=2147483647
            if(num<-2147483648):
                num=-2147483648
           
        except:
            num=0;
        return num
```

