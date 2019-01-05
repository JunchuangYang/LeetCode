## [16. 3Sum Closest](https://leetcode.com/problems/3sum-closest/)

**Medium**

Given an array `nums` of *n* integers and an integer `target`, find three integers in `nums` such that the sum is closest to `target`. Return the sum of the three integers. You may assume that each input would have exactly one solution.

**Example:**

```
Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

题意：给你一个序列和一个目标值，找到三个数的和与目标值最近的和并输出。

思想同第15题一样，使用双指针法，目标值由0换为了target(指定值)。

C++版：

```c++
//Runtime: 12 ms, faster than 37.77% of C++ online submissions for 3Sum Closest.
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(),nums.end());
        int length=nums.size();
        int sum=INT_MAX;
        int res;
        for(int i=0;i<length-1;i++)
        {
            for(int j=i+1,k=length-1;j<k;)
            {
                int sum1=nums[i]+nums[j]+nums[k];
                if(sum1==target)
                {
                    j++;
                    k--;
                }
                else if(sum1>target)
                {
                    k--;
                }
                else
                {
                    j++;
                }
                //判断距离目标值的距离
                if(sum>abs(sum1-target))
                {
                    res=sum1;
                    sum=abs(sum1-target);
                }
           }
        }
        return res;
    }
    
};
```

Python版:

```python
#Runtime: 420 ms, faster than 12.58% of Python3 online submissions for 3Sum Closest.
class Solution:
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        nums.sort()
        res=2147483647 # 最大值
        res1 = target
        length = len(nums)
        for i in range(length):
            j=i+1
            k=length-1
            while j<k:
                sum1 = nums[i]+nums[j]+nums[k]
                if sum1 == target:
                    j+=1
                    k-=1
                elif sum1>target:
                    k-=1
                else:
                    j+=1
                if res > abs(sum1-target):
                    res = abs(sum1 - target)
                    res1 = sum1
        return res1
```

