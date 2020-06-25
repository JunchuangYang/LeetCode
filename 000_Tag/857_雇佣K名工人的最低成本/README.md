# [857. 雇佣 K 名工人的最低成本](https://leetcode-cn.com/problems/minimum-cost-to-hire-k-workers/)


有 `N` 名工人。 第 `i` 名工人的工作质量为 `quality[i]` ，其最低期望工资为 `wage[i]` 。

现在我们想雇佣 `K` 名工人组成一个*工资组。*在雇佣 一组 K 名工人时，我们必须按照下述规则向他们支付工资：

1. 对工资组中的每名工人，应当按其工作质量与同组其他工人的工作质量的比例来支付工资。
2. 工资组中的每名工人至少应当得到他们的最低期望工资。

返回组成一个满足上述条件的工资组至少需要多少钱。

```python
示例 1：

输入： quality = [10,20,5], wage = [70,50,30], K = 2
输出： 105.00000
解释： 我们向 0 号工人支付 70，向 2 号工人支付 35。
示例 2：

输入： quality = [3,1,10,10,1], wage = [4,8,2,2,7], K = 3
输出： 30.66667
解释： 我们向 0 号工人支付 4，向 2 号和 3 号分别支付 13.33333。
 

提示：

1 <= K <= N <= 10000，其中 N = quality.length = wage.length
1 <= quality[i] <= 10000
1 <= wage[i] <= 10000
与正确答案误差在 10^-5 之内的答案将被视为正确的。
```

[官解](<https://leetcode-cn.com/problems/minimum-cost-to-hire-k-workers/solution/gu-yong-k-ming-gong-ren-de-zui-di-cheng-ben-by-lee/>)

```python
class Solution:
    def mincostToHireWorkers(self, quality: List[int], wage: List[int], K: int) -> float:
        ans = []
        for i in range(len(quality)):
            ans.append([wage[i]/quality[i],quality[i]])
        ans.sort(key=lambda x:x[0])
        # 按wage/quality单价值从小到大排序
        # 维护K个quality的大顶堆，求当前i之前最小的k个quality
        heap = []
        nums = float('inf')
        qulit = 0
        for i in range(len(ans)):
            heapq.heappush(heap,-ans[i][1])
            qulit+=ans[i][1]
            if len(heap)>K:
                qulit+=heapq.heappop(heap)
            if len(heap)==K:
                nums = min(ans[i][0]*qulit,nums)
        return nums
```

**没有用heapq，自己手写的大顶堆，速度没有用heapq快。**

```python
class Solution:
    def mincostToHireWorkers(self, quality: List[int], wage: List[int], K: int) -> float:
        ans = []
        for i in range(len(quality)):
            ans.append([wage[i]/quality[i],quality[i]])
        ans.sort(key=lambda x:x[0])
        # 按wage/quality单价值从小到大排序
        # 维护K个quality的大顶堆，求当前i之前最小的k个quality
        heap = []
        nums = float('inf')
        qualit = 0 # i前k个最小的质量
        for i in range(len(ans)):
            if len(heap)<K:
                # 建堆
                heap.append(ans[i][1])
                qualit+=heap[-1]
                n = len(heap)-1
                if n+1 == K:
                    nums = min(nums,qualit*ans[i][0])
                while n>0 and heap[(n-1)//2]<heap[n]:
                    heap[(n-1)//2],heap[n] = heap[n],heap[(n-1)//2]
                    n = (n-1)//2
            else:
                if ans[i][1]<heap[0]:
                    qualit = qualit+ans[i][1]-heap[0]
                    heap[0] = ans[i][1]
                    # 调整堆
                    self.adjust(heap,0)
                nums = min(nums,qualit*ans[i][0])
        return nums
    def adjust(self,heap,root):
        n = len(heap) - 1
        while True:
            child = root*2+1
            if child>n:
                break
            if child+1<=n and heap[child]<heap[child+1]:
                child = child+1
            if heap[root]<heap[child]:
                heap[root],heap[child] = heap[child],heap[root]
                root = child
            else:
                break
```

