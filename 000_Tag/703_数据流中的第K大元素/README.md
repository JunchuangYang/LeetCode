# [703. 数据流中的第K大元素](https://leetcode-cn.com/problems/kth-largest-element-in-a-stream/)

设计一个找到数据流中第K大元素的类（class）。注意是排序后的第K大元素，不是第K个不同的元素。

你的 KthLargest 类需要一个同时接收整数 k 和整数数组nums 的构造器，它包含数据流中的初始元素。每次调用 KthLargest.add，返回当前数据流中第K大的元素。

示例:

int k = 3;

int[] arr = [4,5,8,2];

KthLargest kthLargest = new KthLargest(3, arr);

kthLargest.add(3);   // returns 4

kthLargest.add(5);   // returns 5

kthLargest.add(10);  // returns 5

kthLargest.add(9);   // returns 8

kthLargest.add(4);   // returns 8

说明:

你可以假设 nums 的长度≥ k-1 且k ≥ 1。

**最小堆**

```python
class KthLargest:

    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.nums = nums
        self.heap = []
        for item in nums:
            self.construct_heap(self.heap,item)
    def add(self, val: int) -> int:
        if len(self.heap)<self.k:
            self.construct_heap(self.heap,val)
        elif self.heap[0]<val:
            self.heap[0] = val
            self.adjust(self.heap,0)
        return self.heap[0]
    def construct_heap(self,heap,val):
        if len(self.heap)<self.k:
            self.heap.append(val)
            # 往堆里添加元素
            n = len(self.heap)-1
            while n>0 and self.heap[(n-1)//2] > self.heap[n]:
                self.heap[(n-1)//2] , self.heap[n] = self.heap[n],self.heap[(n-1)//2]
                n = (n-1)//2
        else:
            # 修改堆顶，下沉元素
            if self.heap[0]<val:
                self.heap[0] = val
                self.adjust(self.heap,0)

    def adjust(self,heap,root):
        length = len(heap)-1
        while True:
            child = root*2+1
            if child>length:
                break
            if child+1<=length and heap[child] > heap[child+1]:
                child = child+1
            if heap[root] > heap[child]:
                heap[root] , heap[child] = heap[child],heap[root]
                root = child
            else:
                break 



# Your KthLargest object will be instantiated and called as such:
# obj = KthLargest(k, nums)
# param_1 = obj.add(val)
```

