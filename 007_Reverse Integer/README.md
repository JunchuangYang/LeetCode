###  Reverse Integer

Given a 32-bit signed integer, reverse digits of an integer.

**Example 1:**

```
Input: 123
Output: 321
```

**Example 2:**

```
Input: -123
Output: -321
```

**Example 3:**

```
Input: 120
Output: 21
```

**Note:**
**Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31,  2^31 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.**



题意：题目比较简单，就是将给出的int型的数字反转。注意题目给的提示，**当溢出时返回0.**

C++版：

```c++
class Solution {
public:
    int reverse(int x) {
        int y=0,flag=1;
        long long re=0;
        if(x<0) y=1,x*=(-1);
        while(x!=0)
        {
            long long k=x%10;
            if(k==0&&flag==1)
            {
                x/=10;
                continue;
            }
            flag=0;
            re= re*10+k;
            x/=10;
        }
        if(y) re*=(-1);
        return  (re>INT_MAX || re<INT_MIN) ? 0 : re;
    }
};
```

Python3版：

```python

#32位的整数范围为: -2147483648 ~ 2147483648，因此，正常返回结果的数字需要在-2^31 ~ 2^31范围内。翻转#可以使用字符串来实现。
class Solution:
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        y = str(abs(x))
        num = int(y[::-1])
        if num>2147483648:
            return 0;
        if x<0:
            return -num
        return num
```

**python的切片**真是个神奇的东西。

序列类型是其元素被顺序放置的一种数据结构类型，这种方式允许通过下标的方式来获得某一个数据元素，或者通过指定下标范围来获得一组序列的元素。这种访问序列的方式叫做切片。字符串也可以使用切片操作。切片操作符：[]  , [:]  ,  [::]，调用内置函数slice()函数。



**sequence[index]**

类似于其他语言的数组操作。sequence是序列的名字，index是想要访问的元素对应的偏移量。偏移量正负都可以，-len(sequence)<=index<=len(sequence)-1。正索引以序列的开始为起点，负索引以序列的结束为起点。

试图访问一个越界的索引会引发一个IndexError异常。



**sequence[starting_index:ending_index]**

通过这种方式可以得到从起始索引到结束索引（不包括结束索引所对应的元素）之间的元素，起始索引和结束索引都是可选的，如果没有提供或者用None作为索引值，切片操作会从序列的最开始处开始，或者直到序列的最末尾结束。其中，开始和结束的索引值可以超出字符串长度。

用一个：时，starting_index应该小于ending_index，否则返回空字符串。



**sequence[starting_index:ending_index:step]**

扩展切片操作，step为步长参数，类似range()里的步长参数。

得到的序列从starting_index（包含starting_index）开始，每次以步长前进，即starting_index + step，直到ending_index结束。



有一个经常用到的应用：**翻转字符串**

```
# 输出 'gfedcba'
1 s =s'abcdefg'
2 print s[::-1]
```

参考：[python切片操作](https://www.cnblogs.com/mzct123/p/6031092.html)
