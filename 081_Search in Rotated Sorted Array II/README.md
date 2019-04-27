## [81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

Medium

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,0,1,2,2,5,6]` might become `[2,5,6,0,0,1,2]`).

You are given a target value to search. If found in the array return `true`, otherwise return `false`.

**Example 1:**

```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```

**Example 2:**

```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```

**Follow up:**

- This is a follow up problem to [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/description/), where `nums` may contain duplicates.
- Would this affect the run-time complexity? How and why?

题意比较简单，给你一个排好序的序列，以某一个位置隔开反转一下，从反转后的序列中查找目标元素。找出反转位置，因为已经拍好序了，所以以隔开位置进行两部分二分查找。

C++：

```c++
/*
Runtime: 8 ms, faster than 99.54% of C++ online submissions for Search in Rotated Sorted Array II.
Memory Usage: 8.8 MB, less than 24.27% of C++ online submissions for Search in Rotated Sorted Array II.
*/
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int n = nums.size();
        if(n==0) return false;
        int k=n;
        for (int i=1;i<n;i++)
        {
            if(nums[i-1]>nums[i])
            {
                k = i;
                break;
            }
        }
        return binarySearch(0,k-1,target,nums)||binarySearch(k,n-1,target,nums);
        
    }
    bool binarySearch(int l ,int r,int target,vector<int>& nums)
    {
        while(l<=r)
        {
            int mid = (l+r)/2;
            if(nums[mid] == target) 
                return true;
            else if (nums[mid]>target)
                r = mid-1;
            else
                l = mid+1;
        }
        return false;
    }
};
```

Python3:

用了python中list的count函数，这是一个讨巧的方法，本来就大算算试试，没想到运行时间还挺快。

```python
'''
Runtime: 40 ms, faster than 97.69% of Python3 online submissions for Search in Rotated Sorted Array II.
Memory Usage: 13.4 MB, less than 6.40% of Python3 online submissions for Search in Rotated Sorted Array II.
'''
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        if nums.count(target) ==0 :
            return False
        else:
            return True
```

同样用上述二分查找的方法：

```python
'''
Runtime: 40 ms, faster than 97.69% of Python3 online submissions for Search in Rotated Sorted Array II.
Memory Usage: 13.3 MB, less than 6.40% of Python3 online submissions for Search in Rotated Sorted Array II.
'''
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        n = len(nums)
        if n==0:
            return False
        def binarySearch(l,r,target):
            while l<=r:
                mid = (l+r)//2
                if nums[mid]==target:
                    return True
                elif nums[mid]>target:
                    r = mid-1
                else:
                    l = mid+1
            return False
        k = n
        for i in range(1,n):
            if nums[i-1] > nums[i]:
                k = i
        return binarySearch(0,k-1,target) or binarySearch(k,n-1,target)
    
```

### [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

You are given a target value to search. If found in the array return its index, otherwise return `-1`.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of *O*(log *n*).

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

这到题与上一题很相似，就是返回结果改变，如果找到了返回数组下标，否则返回-1.

其他的一样

c++：

```c++
/*
Runtime: 4 ms, faster than 100.00% of C++ online submissions for Search in Rotated Sorted Array.
Memory Usage: 8.8 MB, less than 98.94% of C++ online submissions for Search in Rotated Sorted Array.
*/
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        if(n==0) return -1;
        int k=n;
        for (int i=1;i<n;i++)
        {
            if(nums[i-1]>nums[i])
            {
                k = i;
                break;
            }
        }
        int res=binarySearch(0,k-1,target,nums);
        if(res!=-1)
            return res;
        return binarySearch(k,n-1,target,nums);
            
        
    }
    int binarySearch(int l ,int r,int target,vector<int>& nums)
    {
        while(l<=r)
        {
            int mid = (l+r)/2;
            if(nums[mid] == target) 
                return mid;
            else if (nums[mid]>target)
                r = mid-1;
            else
                l = mid+1;
        }
        return -1;
    }
};
```

## [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

Medium

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  `[0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2]`).

Find the minimum element.

You may assume no duplicate exists in the array.

**Example 1:**

```
Input: [3,4,5,1,2] 
Output: 1
```

**Example 2:**

```
Input: [4,5,6,7,0,1,2]
Output: 0
```

题意：一个排好的序列，以一个基准点反转，找出这个基准点。

```C++
/*
Runtime: 8 ms, faster than 40.09% of C++ online submissions for Find Minimum in Rotated Sorted Array.
Memory Usage: 8.9 MB, less than 6.75% of C++ online submissions for Find Minimum in Rotated Sorted Array.
*/
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n = nums.size();
        int k=0;
        for (int i=1;i<n;i++)
        {
            if(nums[i-1]>nums[i])
            {
                k = i;
                break;
            }
        }
        return nums[k];
    }
};
```

