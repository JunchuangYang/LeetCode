## [41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

Hard

Given an unsorted integer array, find the smallest missing positive integer.

**Example 1:**

```
Input: [1,2,0]
Output: 3
```

**Example 2:**

```
Input: [3,4,-1,1]
Output: 2
```

**Example 3:**

```
Input: [7,8,9,11,12]
Output: 1
```

**Note:**

Your algorithm should run in *O*(*n*) time and uses constant extra space.



题目大意：给出一个未排序的序列，求出其中缺少的整数

由题意得：所求得结果一定在1~len+1之间（len为字符串长度）。

注意考虑字符串为0的情况

利用标志数组赋值时注意原序列中的数字可能是负数和比较大的整数

C++版：

```c++

class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int len = nums.size();
        int flag[10000] = {0};
        for(int i=0;i<len;i++)
        {
            // 排除负数和大于字符串长度的数
            if(nums[i]>0&&nums[i]<=len)
                flag[nums[i]]=1;
        }
        int k=1;
        while(k<=len)
        {
            if (flag[k]==0)
                return k;
            k++;
        }
        return k;
    }
};
```

Python3:

```py
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        l = len(nums)
        flag = [0] * (l+1) #给列表初始化
        for i in range(l):
            if (nums[i]>0) and (nums[i]<=l):
                flag[nums[i]]=1
        i=1
        while i<=l:
            if flag[i]!=1:
                return i
            i+=1
        return i
        
```

