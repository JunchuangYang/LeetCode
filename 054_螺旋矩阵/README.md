# [54. 螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)

给定一个包含 *m* x *n* 个元素的矩阵（*m* 行, *n* 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

```python
示例 1:

输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
示例 2:

输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]
```

**模拟：[题解](<https://leetcode-cn.com/problems/spiral-matrix/solution/luo-xuan-ju-zhen-by-leetcode-solution/>)**

```python
class Solution(object):
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """
        if not matrix:
            return []
        top,bottom,left,right = 0,len(matrix),0,len(matrix[0])
        res = []
        while top<bottom and left<right:
            for column in range(left,right): #行left->right
                res.append(matrix[top][column])
            for row in range(top+1,bottom): #列top->bottom
                res.append(matrix[row][right-1])
            if left+1<right and top+1<bottom: #防止重复添加
                for column in range(right-2,left,-1):#行right->left
                    res.append(matrix[bottom-1][column])
                for row in range(bottom-1,top,-1): #列bottom->top
                    res.append(matrix[row][left])
            top+=1
            bottom-=1
            left+=1
            right-=1
        return res

```

