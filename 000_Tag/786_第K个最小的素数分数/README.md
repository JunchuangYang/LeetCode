# [786. 第 K 个最小的素数分数](https://leetcode-cn.com/problems/k-th-smallest-prime-fraction/)

一个已排序好的表 A，其包含 1 和其他一些素数.  当列表中的每一个 p<q 时，我们可以构造一个分数 p/q 。

那么第 k 个最小的分数是多少呢?  以整数数组的形式返回你的答案, 这里 answer[0] = p 且 answer[1] = q.

```
示例:
输入: A = [1, 2, 3, 5], K = 3
输出: [2, 5]
解释:
已构造好的分数,排序后如下所示:
1/5, 1/3, 2/5, 1/2, 3/5, 2/3.
很明显第三个最小的分数是 2/5.

输入: A = [1, 7], K = 1
输出: [1, 7]

注意:

A 长度的取值范围在 2 — 2000.
每个 A[i] 的值在 1 —30000.
K 取值范围为 1 —A.length * (A.length - 1) / 2
```

[题解](<https://leetcode-cn.com/problems/k-th-smallest-prime-fraction/solution/di-k-ge-zui-xiao-de-su-shu-fen-shu-by-leetcode/>)

**感觉和丑数有点像。**

使用堆运行时间太慢，也可以用二分做。

使用一个堆记录所有以 `primes[j]` 为分母且未被弹出的最小分数。依次从堆中弹出 `K-1` 个元素，此时堆顶的分数就是结果。

```python
# 4236ms
class Solution:
    def kthSmallestPrimeFraction(self, A: List[int], K: int) -> List[int]:
        heap = []
        for i in range(1,len(A)):
            heap.append([A[0]/A[i],0,i])
        heapq.heapify(heap)
        #print(heap)
        for i in range(K-1):
            node = heapq.heappop(heap)
            if node[1]+1<node[2]:
                heapq.heappush(heap,[A[node[1]+1]/A[node[2]],node[1]+1,node[2]])
            #print(heap)
        h = heapq.heappop(heap)
        return [A[h[1]],A[h[2]]]
```



**使用分数二分，具体细节已在代码中给出**

```python
# 236ms
class Solution:
    def kthSmallestPrimeFraction(self, A: List[int], K: int) -> List[int]:
        # 浮点数二分
        l,r = 0,1.0
        while l<r:
            mid = (l+r)/2.0
            count,x,y = self.under(mid,A)
            if count==K:
                return [x,y]
            elif count>K:
                r=mid
            else:
                l=mid
    def under(self,mid,A):
        count = 0
        max_num = -1
        x,y=-1,-1
        i = 0
        for j in range(1,len(A)):
            num = 0
            # ******重点
            # 循环找比mid小的分数的个数
            # i每次从0开始循环会超时：因为A[j]是递增的，当前A[i]/A[j]>mid跳出后,
            # A[i]/A[j+1]一定比A[i]/A[j]小
            # 所以i前面的A[0...i]/A[j+1]一定小于mid，所以i不用每次从0开始循环。
            while i<len(A) and A[i]<mid*A[j]:
                i+=1
            count+=i
            if i!=0:
                f = A[i-1]/A[j]
                # 记录当前小于mid的最大的分数  
                if f>max_num:
                    max_num = f
                    x,y=A[i-1],A[j]
        return count,x,y
```

