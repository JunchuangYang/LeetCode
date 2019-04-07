## [80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)

Medium

Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that duplicates appeared at most *twice* and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array in-place** with O(1) extra memory.

**Example 1:**

```
Given nums = [1,1,1,2,2,3],

Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.

It doesn't matter what you leave beyond the returned length.
```

**Example 2:**

```
Given nums = [0,0,1,1,1,1,2,3,3],

Your function should return length = 7, with the first seven elements of nums being modified to 0, 0, 1, 1, 2, 3 and 3 respectively.

It doesn't matter what values are set beyond the returned length.
```

**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

题意很清楚，从所给序列中去掉重复次数多余两次的元素，不允许开辟额外的存储空间，只能在原先给的数组上进行操作。

C++：

```c++
/*
Runtime: 12 ms, faster than 100.00% of C++ online submissions for Remove Duplicates from Sorted Array II.
Memory Usage: 8.6 MB, less than 100.00% of C++ online submissions for Remove Duplicates from Sorted Array II.
*/
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        /*
        sum:去重后的数组长度
        f:元素重复的次数
        k:每次去掉重复元素后，在原数组上存放元素的指针
        l:非重复元素的起始位置
        */
        int sum = 0, f=1 ,k=0;
        int l=0;
        if(nums.size()==0)
            return 0;
        for(int i=1;i<nums.size();i++)
        {
            if(nums[i] == nums[l])
            {
                f++;
            }
            else
            {
                if(f>=2) 
                {
                    sum+=2;
                    for(int j=0;j<2;j++)
                        nums[k++]=nums[l];
                }
                else 
                {
                    sum+=1;
                    nums[k++]=nums[l];
                }
                l=i;
                f=1;
            }
        }
        if(f>=2) 
        {
            sum+=2;
            for(int j=0;j<2;j++)
                nums[k++]=nums[l];
        }
        else 
        {
            sum+=1;
            nums[k++]=nums[l];
        }
        return sum;
    }
};
```

Python3版：

在提交区看到的，利用了python中list 的特性

```PYTHON
'''
Runtime: 48 ms, faster than 94.01% of Python3 online submissions for Remove Duplicates from Sorted Array II.
Memory Usage: 13.1 MB, less than 5.43% of Python3 online submissions for Remove Duplicates from Sorted Array II.
'''
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        pos = 2
        while pos < len(nums):
            if nums[pos] == nums[pos-1] and nums[pos] == nums[pos-2]:
                nums.pop(pos)
            else:
                pos += 1
        return len(nums)
```

