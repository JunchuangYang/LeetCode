# [315. 计算右侧小于当前元素的个数](https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self/)

给定一个整数数组 nums，按要求返回一个新数组 counts。数组 counts 有该性质： counts[i] 的值是  nums[i] 右侧小于 nums[i] 的元素的数量。

```python
示例:

输入: [5,2,6,1]
输出: [2,1,1,0] 
解释:
5 的右侧有 2 个更小的元素 (2 和 1).
2 的右侧仅有 1 个更小的元素 (1).
6 的右侧有 1 个更小的元素 (1).
1 的右侧有 0 个更小的元素.
```

**思路:归并排序+索引数组。**

**归并排序求逆序对**

```python
class Solution(object):
    def countSmaller(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        if not nums:
            return []
        self.nums = nums
        self.dic = defaultdict(int)
        index = [i for i in range(len(nums))] # 索引数组
        ans = self.mergeSort(index)
        res = []
        for i in range(len(nums)):
            res.append(self.dic[i])
        return res

    def mergeSort(self,index):
        if len(index) == 1:
            return index
        length = len(index)//2
        index_left= self.mergeSort(index[:length])
        index_right = self.mergeSort(index[length:])
        return self.merge(index_left,index_right)

    def merge(self,index_left,index_right):
        l , r = len(index_left) , len(index_right)
        indexl=indexr=0
        res = []
        while indexl<l and indexr<r:
            if self.nums[index_left[indexl]] > self.nums[index_right[indexr]]:
                res.append(index_left[indexl])
                self.dic[index_left[indexl]]+=r-indexr
                indexl+=1
            else:
                res.append(index_right[indexr])
                indexr+=1
        res.extend(index_left[indexl:])
        res.extend(index_right[indexr:])
        return res
```

