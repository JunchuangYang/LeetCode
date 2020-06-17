# [347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

 

示例 1:

输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
示例 2:

输入: nums = [1], k = 1
输出: [1]

提示：

- 你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。

- 你的算法的时间复杂度必须优于 O(n log n) , n 是数组的大小。
- 题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的。
- 你可以按任意顺序返回答案。

**用dic统计nums中元素的个数，维护容量为K个最小堆**

```python
class Solution(object):
    def topKFrequent(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        # 利用hash计算元素出现的次数，建立一个容量为k的最小堆，取钱k个元素
        dic = defaultdict(int)
        for i in range(len(nums)):
            dic[nums[i]]+=1
        # 建立容量为k的最小堆
        heap = []
        index = 0
        for key in dic.keys():
            if len(heap)<k:
                heap.append([dic[key],key])
                if len(heap) == k: # 建堆
                    for i in range(k//2+1,-1,-1):
                        self.adjustHeap(heap,i)
            else:
                if heap[0][0] < dic[key]:
                    heap[0] = [dic[key],key]
                    self.adjustHeap(heap,0)
        res = []
        for i in range(len(heap)):
            res.append(heap[i][1])
        return res
    
    def adjustHeap(self,heap,i):
        root = i
        while True:
            left = root*2+1
            if left+1<len(heap) and heap[left][0] > heap[left+1][0]:
                left = left+1
            if left>=len(heap):
                break
            if heap[root][0]>heap[left][0]:
                heap[root] ,heap[left] = heap[left],heap[root]
            else:
                break
            root = left
```

