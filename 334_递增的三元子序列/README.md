# [334. 递增的三元子序列](https://leetcode-cn.com/problems/increasing-triplet-subsequence/)

​给定一个未排序的数组，判断这个数组中是否存在长度为 3 的递增子序列。

数学表达式如下:

如果存在这样的 i, j, k,  且满足 0 ≤ i < j < k ≤ n-1，
使得 arr[i] < arr[j] < arr[k] ，返回 true ; 否则返回 false 。
说明: 要求算法的时间复杂度为 O(n)，空间复杂度为 O(1) 。

**示例 1:**

```
输入: [1,2,3,4,5]
输出: true
```

**示例 2:**

```
输入: [5,4,3,2,1]
输出: false
```

**思路：与[300_最长上升子序列](<https://github.com/JunchuangYang/LeetCode/tree/master/300_%E6%9C%80%E9%95%BF%E4%B8%8A%E5%8D%87%E5%AD%90%E5%BA%8F%E5%88%97>)比较像，**

**总之，思想就是让 index1和index2 中存储比较小的元素。这样，index1和index2未必是真实的最长上升子序列，但长度是对的。**

```python
class Solution(object):
    def increasingTriplet(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        index1=index2=0
        n = 1
        for i in range(1,len(nums)):
            if nums[i]>nums[index2]:
                n+=1
                index1 = index2
                index2 = i
            else:
                if nums[i]<=nums[index1]:
                    if n == 1:
                        index1 = index2 = i
                    if n == 2:
                        index1 = i
                else:
                    index2 = i
            if n == 3:
                return True
        return False

```

