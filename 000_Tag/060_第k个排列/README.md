# [60. 第k个排列](https://leetcode-cn.com/problems/permutation-sequence/)

给出集合 [1,2,3,…,n]，其所有元素共有 n! 种排列。

按大小顺序列出所有排列情况，并一一标记，当 n = 3 时, 所有排列如下：

"123"
"132"
"213"
"231"
"312"
"321"
给定 n 和 k，返回第 k 个排列。

说明：

给定 n 的范围是 [1, 9]。
给定 k 的范围是[1,  n!]。
示例 1:

输入: n = 3, k = 3
输出: "213"
示例 2:

输入: n = 4, k = 9
输出: "2314"

[题解](<https://leetcode-cn.com/problems/permutation-sequence/solution/golang-100-faster-by-a-bai-152/>)

**暴力搜索超时**

```python
class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        flag = [i for i in range(1,n+1)]
        index = 1
        res = []
        factorial = [1]
        num = 1
        k=k-1
        for i in range(1,n+1):
            num*=i
            factorial.append(num)
        for i in range(n):
            index = k//(factorial[n-i-1])
            res.append(str(flag[index]))
            flag.pop(index)
            k = k%(factorial[n-i-1])
        return ''.join(res)
```

