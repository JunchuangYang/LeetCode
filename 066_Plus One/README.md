##  [66. Plus One](https://leetcode.com/problems/plus-one/)

Easy

Given a **non-empty** array of digits representing a non-negative integer, plus one to the integer.

The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.

You may assume the integer does not contain any leading zero, except the number 0 itself.

**Example 1:**

```
Input: [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
```

**Example 2:**

```
Input: [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
```

简单模拟题。给你一个序列，最后一个数字加一，将结果输出来。不用考虑首位为0的情况。

C++版：

```c++
/*
Runtime: 4 ms, faster than 100.00% of C++ online submissions for Plus One.
Memory Usage: 8.5 MB, less than 69.94% of C++ online submissions for Plus One.
Next challenges:
*/
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int n = digits.size();
        digits[n-1]+=1;
        if(digits[n-1]==10)
        {
            for(int i=n-1;i>=0;i--)
            {
                
                if(digits[i]==10&&i!=0)
                {
                    digits[i]=0;
                    digits[i-1]+=1;
                }
                if(i==0&&digits[0]==10)
                {
                    vector<int>res;
                    res.push_back(1);
                    for(int j=0;j<n;j++)
                        res.push_back(0);
                    return res;
                }
            }
            return digits;
        }
        else
            return digits;
    }
};
```

Python版：

```python
'''
Runtime: 40 ms, faster than 64.51% of Python3 online submissions for Plus One.
Memory Usage: 13.2 MB, less than 5.29% of Python3 online submissions for Plus One.

'''
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        n = len(digits)
        digits[n-1]+=1
        if digits[n-1]==10:
            k = n-1;
            while k>=1:
                if digits[k]==10:
                    digits[k]=0
                    digits[k-1]+=1
                else:
                    break;
                k-=1
            l=[]
            if k==0 and digits[0]==10:
                for i in range(n+1):
                    if i==0:
                        l.append(1)
                    else:
                        l.append(0)
                return l
            else:
                return digits
        
        else:
            return digits; 
```

上述Python代码没有把Python处理数据的优势发挥出来，下面贴一个在讨论区看到的：

```python
class Solution(object):
    def plusOne(self, digits):
        """
        :type digits: List[int]
        :rtype: List[int]
        """
        # Turn the individual digits into their numerical representation
        # by concatenating them as strings an converting the final result
        # to an integer
        # 将列表list转换成字符串在，将字符串在转换为int
        num = int(reduce(lambda x, y: str(x) + str(y), digits))
        
        # Add one to the number we got from doing the above. Convert it back
        # into a string and perform a list comprehension on the string, 
        # converting each character back into an integer
        # 将加一后的int型数字转换为列表
        return [int(digit) for digit in str(num + 1)]   
```

