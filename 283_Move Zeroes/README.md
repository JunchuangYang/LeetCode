[283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)

Easy

Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Example:**

```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Note**:

1. You must do this **in-place** without making a copy of the array.
2. Minimize the total number of operations.

将序列中的0移到序列最后。

c++

```c++
/*
Runtime: 12 ms, faster than 99.92% of C++ online submissions for Move Zeroes.
Memory Usage: 9.7 MB, less than 10.76% of C++ online submissions for Move Zeroes.
*/
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size();
        int num=0;
        for (int i=0;i<n;i++)
        {
            if(nums[i]==0)
                num++;
            else
            {
                nums[i-num]=nums[i];
            }
        }
        while(num--)
        {
            nums[n-1-num]=0;
        }
    }
};
```

Python:

```python
'''
Runtime: 48 ms, faster than 86.27% of Python3 online submissions for Move Zeroes.
Memory Usage: 14.5 MB, less than 5.21% of Python3 online submissions for Move Zeroes.
'''
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        num=0
        for i in range(n):
            if nums[i]==0:
                num+=1
            else:
                nums[i-num]=nums[i]
        while num:
            nums[n-num] = 0
            num-=1
        

        
```

