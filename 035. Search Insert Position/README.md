## [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/)

Easy

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

**Example 1:**

```
Input: [1,3,5,6], 5
Output: 2
```

**Example 2:**

```
Input: [1,3,5,6], 2
Output: 1
```

**Example 3:**

```
Input: [1,3,5,6], 7
Output: 4
```

**Example 4:**

```
Input: [1,3,5,6], 0
Output: 0
```

题意: 给出一个已经排好序的字符串和目标数字，求出目标数字在字符串中应该放的位置，如果原字符串中已经存在目标数字，返回目标数字在原字符串中的位置。

题意简单，利用二分求出目标数字应该在字符串中的位置，注意处理在字符串开头和结尾的情况。

C++版：

```c++
/**
Runtime: 40 ms, faster than 49.32% of Python3 online submissions for Search Insert Position.
**/
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int begin = 0,end = nums.size()-1;
        int mid = (begin + end) / 2;
        while(begin <end)
        {
            mid = (begin+end)/2;
            if(nums[mid] == target)
            {
                return mid;
            }
            if (nums[mid] > target)
                end = mid - 1;
            else
                begin = mid + 1;
        }
        return nums[begin]>=target?begin:begin+1;
    }
};
```

Python3 版：

```py
#Runtime: 40 ms, faster than 49.32% of Python3 online submissions for Search Insert #Position.
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        begin = 0
        
        end = len(nums)-1
        
        while begin < end:
            mid = int((begin + end)/2)

            if nums[mid] == target:
                return mid
            
            elif nums[mid] > target:
                end = mid - 1
                
            else:
                begin = mid + 1
                
                
        if nums[begin] >= target:
            return begin 
        else:
            return begin +1
```

