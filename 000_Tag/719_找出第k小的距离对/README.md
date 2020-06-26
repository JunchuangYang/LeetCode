# [719. 找出第 k 小的距离对](https://leetcode-cn.com/problems/find-k-th-smallest-pair-distance/)

给定一个整数数组，返回所有数对之间的第 k 个最小**距离**。一对 (A, B) 的距离被定义为 A 和 B 之间的绝对差值。

```
示例 1:

输入：
nums = [1,3,1]
k = 1
输出：0 
解释：
所有数对如下：
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
因此第 1 个最小距离的数对是 (1,1)，它们之间的距离为 0。
提示:

2 <= len(nums) <= 10000.
0 <= nums[i] < 1000000.
1 <= k <= len(nums) * (len(nums) - 1) / 2.

```

**使用堆超时：和[786. 第 K 个最小的素数分数](https://leetcode-cn.com/problems/k-th-smallest-prime-fraction/)的思想是一样的。**

```python
class Solution:
    def smallestDistancePair(self, nums: List[int], k: int) -> int:
        nums.sort()
        heap = []
        for i in range(1,len(nums)):
            heap.append([abs(nums[i]-nums[i-1]),i-1,i])
        # 构建小顶堆
        heapq.heapify(heap)
        for i in range(k-1):
            node = heapq.heappop(heap)
            if node[2]+1<len(nums):
                heapq.heappush(heap,[abs(nums[node[2]+1]-nums[node[1]]),node[1],node[2]+1])
        return heapq.heappop(heap)[0]
```

**二分：将数组排序后，使用二分模拟个数**

```python
class Solution:
    def smallestDistancePair(self, nums: List[int], k: int) -> int:
        nums.sort()
        l,r=nums[0],nums[-1]
        ans = 0
        while l<r:
            mid = l+(r-l)//2
            count,number = self.under(mid,nums)
            if count == k:
                return number
            elif count >k:
                ans = number
                r=mid
            else:
                l=mid+1
        return ans
    def under(self,mid,nums):
        i = 0
        f = 0
        count = 0
        for j in range(1,len(nums)):
            while abs(nums[j]-nums[i])>mid:
                i+=1
            count+= j-i
            f = max(f,nums[j]-nums[i])
        return count,f
```

