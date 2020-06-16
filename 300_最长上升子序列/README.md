# [300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

给定一个无序的整数数组，找到其中最长上升子序列的长度。

示例:

输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
说明:

可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
你算法的时间复杂度应该为 O(n2) 。
进阶: 你能将算法的时间复杂度降低到 O(n log n) 吗?

**动态规划：O（n^2）**

```python
class Solution(object):
    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        dp = [1 for _ in range(len(nums))]
        res = 1
        for i in range(1,len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i],dp[j]+1)
            res = max(dp[i],res)
        return res

```

[题解---动态规划+二分算法](<https://leetcode-cn.com/problems/longest-increasing-subsequence/solution/zui-chang-shang-sheng-zi-xu-lie-dong-tai-gui-hua-e/>)

**动态规划+二分查找**
很具小巧思。

新建数组 s，用于保存最长上升子序列。

对原序列进行遍历，将每位元素二分插入 s中。

- 如果 cell 中元素都比它小，将它插到最后
- 否则，用它覆盖掉比它大的元素中最小的那个。

**总之，思想就是让 s 中存储比较小的元素。这样，s 未必是真实的最长上升子序列，但长度是对的。**

```python
class Solution(object):
    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        s = []
        for i in range(len(nums)):
            if not s:
                s.append(nums[i])
            else:
                if nums[i]>s[-1]:
                    s.append(nums[i])
                else:
                    l = 0
                    r = len(s)-1
                    # 二分,找到在s序列中比当前元素大的最小的那个，进行替换
                    while l<r:
                        mid = (l+r)//2
                        if s[mid]>nums[i]:
                            r = mid
                        elif s[mid] == nums[i]:
                            l = mid
                            break
                        else:
                            l = mid+1
                    s[l] = nums[i] # 替换
                    #print(s)
        return len(s)
```

