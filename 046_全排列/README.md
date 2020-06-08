# [46. 全排列](https://leetcode-cn.com/problems/permutations/)

给定一个 没有重复 数字的序列，返回其所有可能的全排列。

示例:

输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

**深搜：全排列**

```python
class Solution(object):
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        self.flag = [0 for i in range(len(nums))] # 标记
        self.res = [] # 结果
        self.dfs(nums,[])
        return self.res
    
    def dfs(self,nums,temp):
        if len(temp) == len(nums):
            self.res.append(temp[:])
            return 
        for i in range(len(nums)):
            if self.flag[i] == 0:
                temp.append(nums[i])
                self.flag[i] = 1
                self.dfs(nums,temp)
                self.flag[i] = 0
                temp.pop()
        return  

```

