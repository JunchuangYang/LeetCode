# [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

给定一个数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

 

进阶：

你能在线性时间复杂度内解决此题吗？

```
示例:

输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7

提示：

1 <= nums.length <= 10^5
-10^4 <= nums[i] <= 10^4
1 <= k <= nums.length
```

**普通解法超时：**

```python
class Solution(object):
    def maxSlidingWindow(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        res = []
        index = 0
        for i in range(k,len(nums)+1):
            res.append(max(nums[index:i]))
            index+=1
        return res
```

**使用双端队列：队列里保存当前窗口内的最大值**

```python
from collections import deque
class Solution(object):
    def maxSlidingWindow(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        queue = deque()
        if k == 1:
            return nums
        
        ans = []
        for i in range(k):
            self.clean_queue(i,k,queue,nums)
            queue.append(i)
        ans.append(nums[queue[0]])

        for i in range(k,len(nums)):
            self.clean_queue(i,k,queue,nums)
            queue.append(i)
            ans.append(nums[queue[0]])
        return ans

    def clean_queue(self,i,k,queue,nums):
        # 最大值超过滑动窗口的范围：删除
        if queue and  i-queue[0]>=k:
            queue.popleft()

        # 使队列的头元素永远为最大值
        while queue and nums[i]>=nums[queue[-1]]:
            queue.pop()
```

