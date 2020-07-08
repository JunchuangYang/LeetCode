# [31. 下一个排列](https://leetcode-cn.com/problems/next-permutation/)

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须原地修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1

[官解有动图，思路更清晰](<https://leetcode-cn.com/problems/next-permutation/solution/xia-yi-ge-pai-lie-by-leetcode/>)

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        1 5  4 3 2
        """
        left = len(nums)-1
        # 从右向左找到第一次遇见nums[i]>nums[i+1]的i值
        while left>=1 and nums[left] <= nums[left-1]:
            left -= 1
        # 如果left == 0，说明此事字符串降序排列，下一个是第一个升序序列
        if left == 0:
            return nums.sort()
        # 否则，从right开始，找寻比nums[left]大的 最小数进行交换
        right = left
        left -= 1
        res = float('inf')
        index = -1
        while right<len(nums):
            if nums[right]>nums[left]:
                res = min(res,nums[right])
                index = right
            right+=1
        # 交换
        nums[left],nums[index] = nums[index],nums[left]
        # 交换完成后，从left+1开始进行冒泡升序排列
        for i in range(left+1,len(nums)):
            for j in range(i+1,len(nums)):
                if nums[j]<nums[i]:
                    nums[i],nums[j] = nums[j],nums[i]
        return nums
```

