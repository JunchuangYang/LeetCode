##　[73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

Medium

Given a *m* x *n* matrix, if an element is 0, set its entire row and column to 0. Do it [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

```
Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```

**Example 2:**

```
Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
```

**Follow up:**

- A straight forward solution using O(*m**n*) space is probably a bad idea.
- A simple improvement uses O(*m* + *n*) space, but still not the best solution.
- Could you devise a constant space solution?



很容易想到O(m*n)的解法，题目要求要用O(1)的解法

 

参考 ：[LeetCode：Set Matrix Zeroes 矩阵赋零](https://www.cnblogs.com/grandyang/p/4341590.html)



这道题中说的空间复杂度为O(mn)的解法自不用多说，直接新建一个和matrix等大小的矩阵，然后一行一行的扫，只要有0，就将新建的矩阵的对应行全赋0，行扫完再扫列，然后把更新完的矩阵赋给matrix即可，这个算法的空间复杂度太高。

将其优化到O(m+n)的方法是，用一个长度为m的一维数组记录各行中是否有0，用一个长度为n的一维数组记录各列中是否有0，最后直接更新matrix数组即可。

这道题的要求是用O(1)的空间，那么我们就不能新建数组，我们考虑就用原数组的第一行第一列来记录各行各列是否有0.

\- 先扫描第一行第一列，如果有0，则将各自的flag设置为true
\- 然后扫描除去第一行第一列的整个数组，如果有0，则将对应的第一行和第一列的数字赋0
\- 再次遍历除去第一行第一列的整个数组，如果对应的第一行和第一列的数字有一个为0，则将当前值赋0
\- 最后根据第一行第一列的flag来更新第一行第一列

C++版：

```C++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        //O(1)
        int n = matrix.size();
        int m = matrix[0].size();
        int col=0,row=0;
        for(int i=0;i<m;i++)
        {
            if (matrix[0][i]==0)
            {
                col=1;
                break;
            }
        }
        for(int i=0;i<n;i++)
        {  
            if(matrix[i][0]==0)
            {
                row=1;
                break;
            }
        }
        for(int i=1;i<n;i++)
        {
            for(int j=1;j<m;j++)
            {
                if(matrix[i][j]==0)
                {
                    matrix[i][0]=0;
                    matrix[0][j]=0;
                }
            }
        }
        for(int i=1;i<n;i++)
        {
            for(int j=1;j<m;j++)
            {
                if(matrix[i][0]==0||matrix[0][j]==0)
                    matrix[i][j]=0;
            }
        }
        if(row)
        {
            for(int i=0;i<n;i++)
                matrix[i][0]=0;
        }
        if(col)
        {
            for(int i=0;i<m;i++)
                matrix[0][i]=0;
        }
    }
};
```



Python3版：

引用参考博客中一楼fgvlty的评论:

这题用python的话有一个作弊的办法：
用随便一个什么对象，比如一个空list，然后遍历矩阵，把要变成0的位置全部设为这个对象。由于python的语言特性，额外的空间就只有这1个对象，矩阵的元素matrix[i][j]都只是指向这个对象的引用。然后再遍历一次把 is 这个对象的位置全设为0就行了。

```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        obj = []
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                if matrix[i][j]==0:
                    for k in range(len(matrix)):
                        if matrix[k][j] != 0:
                            matrix[k][j] = obj
                    for k in range(len(matrix[0])):
                        if matrix[i][k]!=0:
                            matrix[i][k] = obj
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                if matrix[i][j] is obj:
                    matrix[i][j]=0
```

