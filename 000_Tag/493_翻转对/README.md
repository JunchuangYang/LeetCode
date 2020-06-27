# [493. 翻转对](https://leetcode-cn.com/problems/reverse-pairs/)

给定一个数组 nums ，如果 i < j 且 nums[i] > 2*nums[j] 我们就将 (i, j) 称作一个重要翻转对。

你需要返回给定数组中的重要翻转对的数量。

```python
示例 1:

输入: [1,3,2,3,1]
输出: 2
示例 2:

输入: [2,4,3,5,1]
输出: 3
注意:

给定数组的长度不会超过50000。
输入数组中的所有数字都在32位整数的表示范围内。
```

**归并排序：**

```python
class Solution:
    def reversePairs(self, nums: List[int]) -> int:
        self.count = 0
        res = self.mergeSort(nums)
        #print(res)
        return self.count
    def mergeSort(self,nums):
        if len(nums)<=1:
            return nums
        mid = len(nums)//2
        left = self.mergeSort(nums[:mid])
        right = self.mergeSort(nums[mid:])
        # 不能再归并的时候求逆序，由于判断的是right[j]*2，会出错
        j,n= 0,len(right)
        for i in range(len(left)):
            while j<n and left[i]>right[j]*2:
                j+=1
            self.count +=  j
        return self.merge(left,right)
    
    def merge(self,left,right):
        l,r,ln,rn = 0,0,len(left),len(right)
        ans = []
        #print(left,right)
        while l<ln and r<rn:
            if left[l]>right[r]:
                ans.append(right[r])
                r+=1
            else:
                ans.append(left[l])
                l+=1
        ans.extend(left[l:])
        ans.extend(right[r:])
        return ans
```

