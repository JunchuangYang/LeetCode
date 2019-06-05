[Ugly Number II](<https://leetcode.com/problems/ugly-number-ii/>)

Medium

Write a program to find the `n`-th ugly number.

Ugly numbers are **positive numbers** whose prime factors only include `2, 3, 5`. 

**Example:**

```
Input: n = 10
Output: 12
Explanation: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.
```

**Note:**  

1. `1` is typically treated as an ugly number.
2. `n` **does not exceed 1690**.

参考：<https://www.cnblogs.com/limitlessun/p/8530277.html#_label35>

找到第n个ugly number(质因数只有2,3,5的数字,包括1)

因为ugly number的质因数只有3种可能性,所以每一个ugly number一定是由另一个ugly number乘上这三个数的其中之一得到的(1除外).所以想到了设立3个标志位,分别代表2,3,5的乘数在数组中的位置,判断它们的乘积最小者就是下一个ugly number.

c++：

```c++
/*
Runtime: 52 ms, faster than 20.91% of C++ online submissions for Ugly Number II.
Memory Usage: 8.3 MB, less than 92.13% of C++ online submissions for Ugly Number II.
*/
class Solution {
public:
    int nthUglyNumber(int n) {
        long long ugly_number[n+1] = {0};
        ugly_number[1]=1;
        long long a=1,b=1,c=1;
        for(int i=2;i<=n;i++)
        {
            long long f = min(min(ugly_number[a]*2,ugly_number[b]*3),ugly_number[c]*5);
            if (f == ugly_number[a]*2) a++;
            if (f == ugly_number[b]*3) b++;
            if (f == ugly_number[c]*5)  c++;
            ugly_number[i] = f;
        }
        for(int i =1;i<=n;i++)
        {
            printf("%d,",ugly_number[i]);
        }
        return ugly_number[n];
    }
};
```

Python3:

```python
'''
Runtime: 160 ms, faster than 68.75% of Python3 online submissions for Ugly Number II.
Memory Usage: 13.3 MB, less than 24.42% of Python3 online submissions for Ugly Number II.
'''
class Solution:
    def nthUglyNumber(self, n: int) -> int:
        ugly = []
        ugly.append(1)
        a,b,c = 0,0,0
        for i in range(1,n):
            ugly.append(min(ugly[a]*2,ugly[b]*3,ugly[c]*5))
            if ugly[i] == ugly[a]*2:
                a+=1
            if ugly[i] == ugly[b]*3:
                b+=1 
            if ugly[i] == ugly[c]*5:
                c+=1      
        return ugly[-1]
```

