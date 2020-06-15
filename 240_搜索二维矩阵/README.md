# [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：

每行的元素从左到右升序排列。
每列的元素从上到下升序排列。
示例:

现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
给定 target = 5，返回 true。

给定 target = 20，返回 false。

**二分搜索**

```python
class Solution(object):
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        for i in range(len(matrix)):
            left,right=0,len(matrix[0])-1
            while left<=right:
                mid = (left+right)//2
                if matrix[i][mid] == target:
                    return True
                elif matrix[i][mid] < target:
                    left = mid+1
                else:
                    right = mid-1
        return False
```

**从左下脚开始搜索，速度比上面的二分快**

```python
class Solution(object):
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        # 从左下脚开始进行搜索
        n = len(matrix)
        if n == 0:
            return False
        m = len(matrix[0])-1
        # 左底角坐标
        row,col = n-1,0
        while row>=0 and col<=m:
            if matrix[row][col] == target:
                return True
            elif matrix[row][col] > target:
                row-=1
            else:
                col+=1
        return False
```

