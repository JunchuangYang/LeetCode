## [74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

Medium

Write an efficient algorithm that searches for a value in an *m* x *n* matrix. This matrix has the following properties:

- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.

**Example 1:**

```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true
```

**Example 2:**

```
Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false
```

题目大意：给出一个目标数字，判断是否在所给的序列中。二分查找，注意判断为空的情况。

该开始我使用的是两个二分，第一个二分查找所在的行，然后再使用第二个二分查找在所在的行中的情况。一直wa，最后将第一个二分取消，改成for循环遍历，比较每行的最后一个与目标值的大小。

C++版：

```c++
/*
Runtime: 12 ms
Memory Usage: 9.9 MB
*/
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int n = matrix.size();
        if(n==0) return false;
        int m = matrix[0].size();
        if(m==0) return false;
        int l=0,r=m-1,flag=0,mid;
        for (int i=0;i<n;i++)
        {
            if (matrix[i][m-1]>=target)
                while(l<=r)
                {
                    mid = (l+r)/2;
                    if (matrix[i][mid]==target)
                    {
                        flag=1;
                        break;
                    }
                    else if(matrix[i][mid]>target)
                    {
                        r=mid-1;
                    }
                    else
                        l=mid+1;
                }
            if(flag) break;
        }
        if(flag) return true;
        else return false; 
    }
};
```

Python3版：

```python
'''
Runtime: 36 ms, faster than 99.48% of Python3 online submissions for Search a 2D Matrix.
Memory Usage: 14 MB, less than 5.71% of Python3 online submissions for Search a 2D Matrix.
'''
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        n = len(matrix)
        if n == 0: return False
        m = len(matrix[0])
        if m == 0: return False
        flag = False
        for i in range(n):
            if matrix[i][-1]>=target:
                l = 0
                r = m-1
                while l<=r:
                    mid = int((l+r)/2)
                    if matrix[i][mid] == target:
                        flag = True
                        break
                    elif matrix[i][mid] > target:
                        r = mid - 1
                    else:
                        l = mid + 1
            if flag :
                break
        return flag
```

