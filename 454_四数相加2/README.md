# [454. 四数相加 II](https://leetcode-cn.com/problems/4sum-ii/)

给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 (i, j, k, l) ，使得 A[i] + B[j] + C[k] + D[l] = 0。

为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -2^28 到 2^28 - 1 之间，最终结果不会超过 2^31 - 1 。

```python
例如:

输入:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]

输出:
2

解释:
两个元组如下:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
```

[思路](<https://leetcode-cn.com/problems/4sum-ii/solution/jiang-si-shu-zhi-he-zhuan-hua-wei-liang-ge-liang-s/>)

思路：
初次看到这个题，感觉只能四重 for 循环来解决，结果不出意外超时。

再次分析后发现，我们可以采用两数之和的思路，思考思路如下所示：

- 先统计出 A 和 B 两个数组中各种 sum 的个数的统计 hash 表 dictx ；
- 再统计出 C 和 D 两个数组中各种 sum 的个数的统计 hash 表 dicty ；
- 参考两数之和的思路，遍历 dictx 表，在 dicty 中找到对应 key 的个数，然后统计出两个表中的个数乘积；
- 遍历完 first 之后的结果即是最终的结果。

```python
class Solution(object):
    def fourSumCount(self, A, B, C, D):
        """
        :type A: List[int]
        :type B: List[int]
        :type C: List[int]
        :type D: List[int]
        :rtype: int
        """
        dicx=defaultdict(int)
        dicy=defaultdict(int)
        for i in range(len(A)):
            for j in range(len(B)):
                dicx[A[i]+B[j]]+=1
                dicy[C[i]+D[j]]+=1
        res = 0
        for key in dicx.keys():
            ans = 0-key
            res += dicx[key] * dicy[ans]
        return res
```

