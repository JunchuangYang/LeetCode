### 5. Longest Palindromic Substring

Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

**Example 1:**

```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2:**

```
Input: "cbbd"
Output: "bb"
```

题意：从一个给定的字符串中找出最长的**回文字符串**。

题意很简单明了，刚开始想了一下，不能暴力，会超时。然后开始想从中间往两边找，设置两个指针，左边一个右边一个，好像可行。然后就陷入到了回文串为单数和双数的问题中（如aa，aba中），这时指针遍历的起点是不一样的。最后没忍住搜了一下题解，解法是单数双数都尝试一下，开始时字符为回文串的中心向两边开始遍历。

### C++：时间：O(N2)  ,  空间:O(1)

```c++
class Solution {

public :
    //最大长度，回文串开始位置，结束位置
    int maxsum=-1,bstr=-1,estr=-1;
    string longestPalindrome(string s) {
        int len=s.length();
        int lstr=len-1;
        if(len<2) return s;
        for(int i=lstr;i>=0;i--)
        {
            //尝试单数和双数的情况，最后一位为标志位
            find(s,i,i,1);
            find(s,i,i+1,0);
        }
        return s.substr(bstr,maxsum);
    }
    
    void find(string s,int i,int j,int flag)
    {
        int sum=0;
        while(s[i]==s[j]&&i>=0&&j<s.length())
        {
            i--;
            j++;
            sum+=2;
        }
        //如果为单数情况，长度减一
        if(flag) sum--;
        if(sum>maxsum)
        {
            maxsum=sum;
            bstr=i+1;
            estr=j-1;
        }
    }
};
```

还有一个可以将复杂度降到O(N)的算法：http://www.cnblogs.com/bitzhuwei/p/Longest-Palindromic-Substring-Part-II.html