## [118. Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/)

Easy

Given a non-negative integer *numRows*, generate the first *numRows* of Pascal's triangle.

![img](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)
In Pascal's triangle, each number is the sum of the two numbers directly above it.

**Example:**

```
Input: 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

**杨辉三角**

C++版：

```c++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> res;
        for(int i = 0;i<numRows;i++)
        {
            vector<int> res2;
            res2.push_back(1);
            for(int j=0;j<i-1;j++)
            {
                res2.push_back(res[i-1][j]+res[i-1][j+1]);
            }
            if(i!=0)
                res2.push_back(1);
            res.push_back(res2);
        }
        return res;
    }
};
```

Python3版：

```python
class Solution:
    def generate(self, numRows: int) -> List[List[int]]:
        res = []
        for i in range(numRows):
            r = []
            r.append(1)
            for j in range(i-1):
                r.append(res[i-1][j]+res[i-1][j+1])
            if i>0:
                r.append(1)
            res.append(r)
        return res
```

## [119. Pascal's Triangle II](https://leetcode.com/problems/pascals-triangle-ii/)

根118一样的解法：打表输出

```c++
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<vector<int>> res;
        for(int i = 0;i<=33;i++)
        {
            vector<int> res2;
            res2.push_back(1);
            for(int j=0;j<i-1;j++)
            {
                res2.push_back(res[i-1][j]+res[i-1][j+1]);
            }
            if(i!=0)
                res2.push_back(1);
            res.push_back(res2);
        }
        vector<int> r;
        for(int j=0;j<res[rowIndex].size();j++)
            r.push_back(res[rowIndex][j]);
        return r;
    }
};
```

