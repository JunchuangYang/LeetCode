# [56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)

给出一个区间的集合，请合并所有重叠的区间。

示例 1:

输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
示例 2:

输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。

```python
class Solution(object):
    def merge(self, intervals):
        """
        :type intervals: List[List[int]]
        :rtype: List[List[int]]
        """
        if not intervals:
            return []
        intervals = sorted(intervals,key=lambda x:(x[0],-x[1]))
        res = []
        begin = intervals[0][0]
        end = intervals[0][1]
        for item in intervals:
            x,y = item[0],item[1]

            if x>=begin and x<=end:
                if y<=end: # 区间内
                    continue
                else:
                    end = y
            else: # x>end
                res.append([begin,end])
                begin = x
                end =y
        res.append([begin,end])
        return res
```

**官解：**

```python
class Solution(object):
    def merge(self, intervals):
        """
        :type intervals: List[List[int]]
        :rtype: List[List[int]]
        """
        if not intervals:
            return []
        intervals = sorted(intervals,key=lambda x:(x[0],-x[1]))
        res = []
        for item in intervals:
             # 如果列表为空，或者当前区间与上一区间不重合，直接添加
            if not res or res[-1][1] < item[0]:
                res.append(item)
            else:
                 # 否则的话，我们就可以与上一区间进行合并
                res[-1][1] = max(res[-1][1],item[1])
        return res
```

