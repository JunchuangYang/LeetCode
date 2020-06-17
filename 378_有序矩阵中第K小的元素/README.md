# [378. 有序矩阵中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)

给定一个 *n x n* 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第 `k` 小的元素。
请注意，它是排序后的第 `k` 小元素，而不是第 `k` 个不同的元素。

示例：

matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

返回 13。

提示：
你可以假设 k 的值永远是有效的，1 ≤ k ≤ n^2.

**归并排序：效率不高**

```python
class Solution(object):
    def kthSmallest(self, matrix, k):
        """
        :type matrix: List[List[int]]
        :type k: int
        :rtype: int
        """
        res = []
        for i in range(0,len(matrix)):
            if i == 0:
                res.extend(matrix[i])
            else:
                res = self.mergeSort(res,matrix[i])
        #print(res)
        return res[k-1]
    def mergeSort(self,res,s):
        startr = starts= 0
        ans = []
        while startr<len(res) and starts<len(s):
            if res[startr] < s[starts]:
                ans.append(res[startr])
                startr+=1
            else:
                ans.append(s[starts])
                starts+=1
        ans.extend(res[startr:])
        ans.extend(s[starts:])
        return ans
```

**值域二分：[题解](<https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/solution/zhi-yu-er-fen-fa-chao-xiang-xi-nai-xin-jiang-jie-b/>)**

首先第k大数一定落在[l, r]中，其中l = matrix\[0][0], r = matrix\[row - 1][col - 1].
我们二分值域[l, r]区间，mid = (l + r) >> 1, 对于mid，我们检查矩阵中有多少元素小于等于mid，
记个数为cnt，那么有：

1、如果cnt < k, 那么[l, mid]中包含矩阵元素个数一定小于k，那么第k小元素一定不在[l, mid]
中，必定在[mid + 1, r]中，所以更新l = mid + 1.

2、否则cnt >= k，那么[l, mid]中包含矩阵元素个数就大于等于k，即第k小元素一定在[l,mid]区间中，
更新r = mid;

至于怎么得矩阵中有多少元素小于等于k，可以利用矩阵本身性质，从左下角开始按列枚举，具体可参考代码。

综上：
算法时间复杂度为O(n * log(m)), 其中n = max(row, col)，代表矩阵行数和列数的最大值,
 m代表二分区间的长度，即矩阵最大值和最小值的差。

```python
class Solution(object):
    def kthSmallest(self, matrix, k):
        """
        :type matrix: List[List[int]]
        :type k: int
        :rtype: int
        """
        n,m = len(matrix)-1,len(matrix[0])-1
        left , right = matrix[0][0] , matrix[n][m]
        while left < right:
            mid = left + (right-left)//2
            num = self.binarySort(mid,matrix,n,m)
            if num<k:
                left = mid+1
            else:
                right = mid
        return right
    def binarySort(self,mid,matrix,n,m):
        num = 0
        x,y=n,0
        while x>=0 and y<=m:
            if matrix[x][y] <= mid:
                num += x+1
                y+=1
            else:
                x-=1
        return num
```

