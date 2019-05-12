[228. Summary Ranges](https://leetcode.com/problems/summary-ranges/)

Medium

Given a sorted integer array without duplicates, return the summary of its ranges.

**Example 1:**

```
Input:  [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: 0,1,2 form a continuous range; 4,5 form a continuous range.
```

**Example 2:**

```
Input:  [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: 2,3,4 form a continuous range; 8,9 form a continuous range.
```

题意：在一个序列中找到连续的序列记录下来。

C++版：

```c++
/*
Runtime: 0 ms, faster than 100.00% of C++ online submissions for Summary Ranges.
Memory Usage: 8.5 MB, less than 90.83% of C++ online submissions for Summary Ranges.
*/

class Solution {
public:
    vector<string> summaryRanges(vector<int>& nums) {
        int n = nums.size();
        vector<string> res;
        if(n==0) return res;
        string s;
        int num=1,pre = nums[0];
        s = to_string(pre);
        for(int i=1;i<n;i++)
        {
            if(nums[i]==pre+1)
            {
                num++;
                pre=nums[i];
                continue;
            }
            if(num!=1)
            {
                s+="->"+to_string(pre);
            }
            res.push_back(s);
            s=to_string(nums[i]);
            pre = nums[i];
            num=1;
        }
        if(num!=1)
        {
            s+="->"+to_string(pre);
        }
        res.push_back(s);
        return res;
    }
};
```

Python3:

```python
'''
Runtime: 36 ms, faster than 78.51% of Python3 online submissions for Summary Ranges.
Memory Usage: 13 MB, less than 5.19% of Python3 online submissions for Summary Ranges.
'''
class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        n = len(nums)
        res = []
        start = 0
        end = 0
        for i in range(n):
            if i == 0 or nums[i-1] + 1 != nums[i]:
                start = nums[i]
            if i == n-1 or nums[i+1] -1 != nums[i]:
                end = nums[i]
                if start == end:
                    res.append(str(start))
                else:
                    res.append(str(start) + '->' + str(end))
        return res
```

