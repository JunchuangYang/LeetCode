# [1504. 统计全 1 子矩形](https://leetcode-cn.com/problems/count-submatrices-with-all-ones/)

给你一个只包含 0 和 1 的 rows * columns 矩阵 mat ，请你返回有多少个 子矩形 的元素全部都是 1 。

 

示例 1：

输入：mat = [[1,0,1],
​            [1,1,0],
​            [1,1,0]]
输出：13
解释：
有 6 个 1x1 的矩形。
有 2 个 1x2 的矩形。
有 3 个 2x1 的矩形。
有 1 个 2x2 的矩形。
有 1 个 3x1 的矩形。
矩形数目总共 = 6 + 2 + 3 + 1 + 1 = 13 。
示例 2：

输入：mat = [[0,1,1,0],
​            [0,1,1,1],
​            [1,1,1,0]]
输出：24
解释：
有 8 个 1x1 的子矩形。
有 5 个 1x2 的子矩形。
有 2 个 1x3 的子矩形。
有 4 个 2x1 的子矩形。
有 2 个 2x2 的子矩形。
有 2 个 3x1 的子矩形。
有 1 个 3x2 的子矩形。
矩形数目总共 = 8 + 5 + 2 + 4 + 2 + 2 + 1 = 24 。
示例 3：

输入：mat = [[1,1,1,1,1,1]]
输出：21
示例 4：

输入：mat = [[1,0,1],[0,1,0],[1,0,1]]
输出：5


提示：

1 <= rows <= 150
1 <= columns <= 150
0 <= mat\[i][j] <= 1

[统计+压缩](<https://leetcode-cn.com/problems/count-submatrices-with-all-ones/solution/bian-tong-ji-bian-ya-suo-xiang-xi-by-quantum-10/>)

```python
class Solution:
    def numSubmat(self, mat: List[List[int]]) -> int:
        ans = 0
        rows,columns = len(mat),len(mat[0])
        for i in range(rows):
            for j in range(i,rows):
                num = 0
                for k in range(columns):
                    if mat[j][k]==0:
                        num = 0
                    else:
                        num+= 1 if k==0 or mat[j][k]==1 else 0
                    ans += num
            
            for j in range(rows-1,i,-1):
                for k in range(columns):
                    mat[j][k] &= mat[j-1][k] 
        return ans
```

