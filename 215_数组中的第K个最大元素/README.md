# [215. 数组中的第K个最大元素](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/)

在未排序的数组中找到第 **k** 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

```
示例 1:

输入: [3,2,1,5,6,4] 和 k = 2
输出: 5

示例 2:

输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```

**思路一：快速排序寻找TOPK**

```python
class Solution(object):
    def findKthLargest(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        return self.partition(nums,0,len(nums)-1,k)
        
    def partition(self,nums,start,end,k):
        index = self.partition_sort(nums,start,end)
        if index == k-1:
            return nums[index]
        elif index > k-1:
            return self.partition(nums,start,index-1,k)
        else:
            return self.partition(nums,index+1,end,k)

    def partition_sort(self,nums,start,end):
        pivot = nums[start]
        # 从大到小排列
        while start<end:
            while start<end and nums[end]<=pivot:
                end-=1
            nums[start] = nums[end]
            while start<end and nums[start]>pivot:
                start+=1
            nums[end] = nums[start]
        nums[start] = pivot
        return start
```

**思路二：堆，维护一个长度为K的小顶堆，堆顶元素为第TOPK**

```python
class Solution(object):
    def findKthLargest(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        heaplist = nums[0:k]
        # 构建小顶堆
        # 从堆的第一个非叶子节点开始
        length = len(heaplist)//2
        for i in range(length,-1,-1):
            self.adjust_heap(heaplist,i,len(heaplist)-1)
        for j in range(k,len(nums)):
            if nums[j]>heaplist[0]:
                # 比堆顶值大，入堆
                heaplist[0] = nums[j]
                self.adjust_heap(heaplist,0,len(heaplist)-1)
        return heaplist[0]
    
    def adjust_heap(self,heaplist,start,end):
        root = start
        while True:
            child = root*2+1 # 左节点
            if child>end: 
                break
            # 寻找左右节点最小值
            if child+1<=end and heaplist[child] > heaplist[child+1]:
                child = child+1
            # 交换
            if heaplist[root] > heaplist[child]:
                heaplist[root] , heaplist[child] = heaplist[child],heaplist[root]
                root = child
            else:
                break
```

