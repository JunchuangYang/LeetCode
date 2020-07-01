# [718. 最长重复子数组](https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/)

给两个整数数组 `A` 和 `B` ，返回两个数组中公共的、长度最长的子数组的长度。

```
示例 1:

输入:
A: [1,2,3,2,1]
B: [3,2,1,4,7]
输出: 3
解释: 
长度最长的公共子数组是 [3, 2, 1]。
说明:

1 <= len(A), len(B) <= 1000
0 <= A[i], B[i] < 100
```

[题解](<https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/solution/zui-chang-zhong-fu-zi-shu-zu-by-leetcode-solution/>)

**动态规划**

```python
class Solution:
    def findLength(self, A: List[int], B: List[int]) -> int:
        dp = [[0]*(len(A)+1) for _ in range(len(B)+1)]
        ans = 0
        for i in range(len(A)-1,-1,-1):
            for j in range(len(B)-1,-1,-1):
                if A[i]==B[j]:
                    dp[i][j] = dp[i+1][j+1]+1
                ans = max(ans,dp[i][j])
        return ans
```

**滑动窗口**

```python
class Solution:
    def findLength(self, A: List[int], B: List[int]) -> int:
        ans = 0
        lenA,lenB = len(A),len(B)
        for i in range(lenA):
            ans = max(ans,self.maxLength(A,i,B,0,min(lenA-i,lenB)))
        for i in range(lenB):
            ans = max(ans,self.maxLength(A,0,B,i,min(lenA,lenB-i)))
        return ans
    def maxLength(self,A,iA,B,iB,length):
        ans = 0
        k=0
        for i in range(length):
            if A[iA+i] == B[iB+i]:
                k+=1
                ans = max(ans,k)
            else:
                k=0
        return ans
```

