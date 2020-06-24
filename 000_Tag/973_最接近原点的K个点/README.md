# [973. 最接近原点的 K 个点](https://leetcode-cn.com/problems/k-closest-points-to-origin/)

我们有一个由平面上的点组成的列表 points。需要从中找出 K 个距离原点 (0, 0) 最近的点。

（这里，平面上两点之间的距离是欧几里德距离。）

你可以按任何顺序返回答案。除了点坐标的顺序之外，答案确保是唯一的。

```
示例 1：

输入：points = [[1,3],[-2,2]], K = 1
输出：[[-2,2]]
解释： 
(1, 3) 和原点之间的距离为 sqrt(10)，
(-2, 2) 和原点之间的距离为 sqrt(8)，
由于 sqrt(8) < sqrt(10)，(-2, 2) 离原点更近。
我们只需要距离原点最近的 K = 1 个点，所以答案就是 [[-2,2]]。
示例 2：

输入：points = [[3,3],[5,-1],[-2,4]], K = 2
输出：[[3,3],[-2,4]]
（答案 [[-2,4],[3,3]] 也会被接受。）
 

提示：

1 <= K <= points.length <= 10000
-10000 < points[i][0] < 10000
-10000 < points[i][1] < 10000
```

**构建最大堆找离原点最近的K个点。**

```python
# 1076 ms
class Solution:
    def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:
        dic = defaultdict(list)
        heap = [] # 大顶堆
        for item in points:
            dist = item[0]**2+item[1]**2
            # 建立大顶堆
            if len(heap)<K:
                heap.append([dist,item])
                n = len(heap)-1
                while n>0 and heap[(n-1)//2][0]<heap[n][0]:
                    heap[(n-1)//2],heap[n] = heap[n],heap[(n-1)//2]
                    n = (n-1)//2
            else:
                if dist<heap[0][0]:
                    heap[0] = [dist,item]
                    self.adjust(heap,0)
        ans = []
        for item in heap:
            ans.append(item[1])
        return ans
    def adjust(self,heap,root):
        n = len(heap)-1
        while True:
            child = root*2+1
            if child>n:
                break
            if child+1<=n and heap[child][0] < heap[child+1][0]:
                child = child+1
            if heap[root][0] < heap[child][0]:
                heap[root],heap[child] = heap[child],heap[root]
                root = child
            else:
                break
```

**python中的heapq**

```python
# 868ms
class Solution:
    def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:
        res = []
        for item in points:
            res.append([item[0]**2+item[1]**2,item])
        ans = nsmallest(K,res)
        return [item[1] for item in ans]
```

**sort排序**

```python
# 816ms
class Solution:
    def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:
        points.sort(key=lambda x:x[0]**2+x[1]**2)
        return points[:K]
```

