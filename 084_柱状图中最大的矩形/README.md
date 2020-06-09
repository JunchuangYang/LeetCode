# [84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 ![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/histogram.png)

以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 [2,1,5,6,2,3]。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/histogram_area.png) 

图中阴影部分为所能勾勒出的最大矩形面积，其面积为 10 个单位。



示例:

输入: [2,1,5,6,2,3]
输出: 10



**暴力超时：**

```python
class Solution(object):
    def largestRectangleArea(self, heights):
        """
        :type heights: List[int]
        :rtype: int
        """
        left ,right = 0,len(heights)-1
        res = 0
        for i in range(len(heights)):
            minn = heights[i]
            for j in range(i,len(heights)):
                minn = min(minn,heights[j])
                res = max(res,minn*(j-i+1))
        return res
```

**看了题解，使用单调栈。**

[题解](<https://leetcode-cn.com/problems/largest-rectangle-in-histogram/solution/bao-li-jie-fa-zhan-by-liweiwei1419/>)

```python
class Solution(object):
    def largestRectangleArea(self, heights):
        """
        :type heights: List[int]
        :rtype: int
        """
        res = 0
        stack = [] # 维护一个单调严格递增栈
        for i in range(len(heights)):
            if not stack:
                stack.append(i)
            else:
                if heights[stack[-1]] < heights[i]:
                    stack.append(i)
                else:
                    while len(stack)>0 and heights[stack[-1]] >= heights[i]:
                        if len(stack)>1:
                            # 找出第二个栈顶和当前i的距离，为栈顶元素的宽度
                            res = max(res, heights[stack[-1]] * (i-stack[-2]-1))
                        if len(stack) == 1:
                            # 往左计算
                            res = max(res, heights[stack[-1]] * i)
                        stack.pop()
                    stack.append(i)
        while len(stack)>0:
            if len(stack) == 1:
                # 当前高度为数组中最小，宽度为元素个数
                res = max(res,len(heights) * heights[stack[-1]])
            else:
                 # 宽度为数组中元素个数减去次栈顶的坐标
                res = max(res,(len(heights) - stack[-2]-1)*heights[stack[-1]])
            stack.pop()
        return res
```

