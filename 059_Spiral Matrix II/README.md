## [59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/)

Medium

Given a positive integer *n*, generate a square matrix filled with elements from 1 to *n*2 in spiral order.

**Example:**

```
Input: 3
Output:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
```

模拟

```C++

/*
Runtime: 4 ms, faster than 100.00% of C++ online submissions for Spiral Matrix II.
Memory Usage: 9.2 MB, less than 5.15% of C++ online submissions for Spiral Matrix II.
*/
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        int mat[n+1][n+1];
        int flag=1;
        int sum=1;
        int i=1,j=1,l=1,r=n;
        while(sum<=n*n)
        {
            mat[i][j]=sum;
            if(i==l+1&&j==l)//到达了每层转换方向处
            {
                l++;
                r--;
                i=l;
                j=l;
                flag=1;
                sum++;
                continue;
            }
            if(flag==1) j++;//横+
            else if (flag==2) i++;//竖+
            else if (flag==3) j--;//横-
            else  i--;//竖-
            
            if(j>r) flag=2,j=r,i++;//横+处转换方向，变竖+
            else if(i>r) flag=3,i=r,j--;//竖+处转换方向，横-
            else if(j<l) flag=4,j=l,i--;//横-处转换方向，竖-

            sum++;
        }
        vector<vector<int>> s;
    
        for(int i=1;i<=n;i++)
        {
            vector<int> k;
            for(int j=1;j<=n;j++)
                k.push_back(mat[i][j]);
            s.push_back(k);
        }
        
        return s;
    }
};
```

