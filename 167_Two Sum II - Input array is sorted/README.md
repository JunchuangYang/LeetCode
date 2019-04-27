## [167. Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

Easy

Given an array of integers that is already **sorted in ascending order**, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

**Note:**

- Your returned answers (both index1 and index2) are not zero-based.
- You may assume that each input would have *exactly* one solution and you may not use the *same* element twice.

**Example:**

```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```

题意：从一个排好序的序列中找出两个数的和等于目标值。

思想：在序列前后各放一个指针i，j，入两数的和大于目标值，则j--；若小于，则i++；若等于则输出结果。

```c++
/*
Runtime: 8 ms, faster than 99.11% of C++ online submissions for Two Sum II - Input array is sorted.
Memory Usage: 9.4 MB, less than 85.17% of C++ online submissions for Two Sum II - Input array is sorted.
*/
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int n = numbers.size();
        int i=0,j=n-1;
        while(i<j)
        {
            if (numbers[i] + numbers[j] == target)
                break;
            else if (numbers[i] + numbers[j] > target)
                j--;
            else 
                i++;
        }
        vector<int> res;
        res.push_back(i+1);
        res.push_back(j+1);
        return  res;
    }
};
```

