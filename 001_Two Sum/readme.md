
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
给出一组序列，和目标值，从序列中找出两数相加等于目标值，返回两数的下标。
注意：你不能使用同一个元素两次

```python
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        L=[]
        for x in range(0,len(nums)):
            for y in range(x+1,len(nums)):
                if nums[x]+nums[y]==target:
                    L.append(x)
                    L.append(y)
                    break
            if len(L)==2:
                break
        return L
```
上面的这个版本是我写的第一次的版本，用的是暴力枚举的写法，时间复杂度较高


优化版：

思想：构建一个h[]，将nums数组中的target-value当做h中的index，此时的h[target-value]值为value在nums下的坐标

```python
class Solution:
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        
        h={} #字典
        for i in range(len(nums)):
            if nums[i] in h:
                return [h[nums[i]],i]
            h[target-nums[i]]=i
```


