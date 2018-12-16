## [Palindrome Number](https://leetcode.com/problems/palindrome-number/)

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

**Example 1:**

```
Input: 121
Output: true
```

**Example 2:**

```
Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

**Example 3:**

```
Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

题意：给你一个数字，判断它是否是回文串。比较简单。

C++版：这是我自己写的，判断完特殊情况后在逐一进行判断。

```c++
class Solution {
public:
    bool isPalindrome(int x) {
        if(x<0) return false;
        if(x>=0&&x<=9) return true;
        int s[20];
        int y=0;
        while(x!=0)
        {
            s[y++]=x%10;
            x/=10;
        }
        for(int i =0;i<y/2;i++)
        {
            if(s[i]!=s[y-i-1])
                return false;
        }
        return true;
    }
};
```

在提交区看到的代码：

根据回文串的特性，判断从后往前相乘得出的结果是否与原数字相等。

```c++
class Solution {
public:
bool isPalindrome(int x) {
	if (x < 0) return false;
	int t = x, y = 0;
	while (t) {
		y *= 10;
		y += t % 10;
		t /= 10;
	}
	return y == x;
}
};
```

Python3版：

```python
class Solution:
    def isPalindrome(self, x):
        """
        :type x: int
        :rtype: bool
        """
        if x<0:
            return False
        s = str(x)
        l = int(len(s)/2)
        for i in range(l):
            if s[i] != s[-(i+1)]:
                return False
        return True
```

在提交区看到的代码，很好的利用了切片。

```python
class Solution:
    def isPalindrome(self, x):
        """
        :type x: int
        :rtype: bool
        """
        return str(x)==str(x)[::-1]

```

