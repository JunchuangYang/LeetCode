# [面试题 17.20. 连续中值](https://leetcode-cn.com/problems/continuous-median-lcci/)

随机产生数字并传递给一个方法。你能否完成这个方法，在每次产生新值时，寻找当前所有值的中间值（中位数）并保存。

中位数是有序列表中间的数。如果列表长度是偶数，中位数则是中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

- void addNum(int num) - 从数据流中添加一个整数到数据结构中。

- double findMedian() - 返回目前所有元素的中位数。

**示例：**

```
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
```

**使用模块heapq建造小顶堆和大顶堆**

```python
class MedianFinder:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.smallheap = []
        self.bigheap = []
    def addNum(self, num: int) -> None:
        # 维护前k个元素的大顶堆和后n-k个元素的小顶堆
        # 由于模块heap实现的是小顶堆，所以在建造大顶堆的时候将加进去的数字取反
        if len(self.smallheap) == len(self.bigheap):
            if not self.bigheap:
                heapq.heappush(self.bigheap,-num)
            else:
                if num>self.smallheap[0]:
                    top = heapq.heappop(self.smallheap)
                    heapq.heappush(self.smallheap,num)
                    heapq.heappush(self.bigheap,-top)
                else:
                    heapq.heappush(self.bigheap,-num)
        else:
            if num < -self.bigheap[0]:
                top = heapq.heappop(self.bigheap)
                heapq.heappush(self.bigheap,-num)
                heapq.heappush(self.smallheap,-top)
            else:
                heapq.heappush(self.smallheap,num)
    def findMedian(self) -> float:
        if (len(self.bigheap) + len(self.smallheap))%2==0:
            return (-self.bigheap[0]+self.smallheap[0])/2
        else:
            return -self.bigheap[0]


# Your MedianFinder object will be instantiated and called as such:
# obj = MedianFinder()
# obj.addNum(num)
# param_2 = obj.findMedian()
```

