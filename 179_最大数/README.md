# [179. 最大数](https://leetcode-cn.com/problems/largest-number/)

给定一组非负整数，重新排列它们的顺序使之组成一个最大的整数。

```
示例 1:

输入: [10,2]
输出: 210

示例 2:

输入: [3,30,34,5,9]
输出: 9534330
```

显而易见，要重新定义两个数的大小，排好序之后按顺序将字符串拼接起来即可。

如何定义两个数的大小，不太容易立刻想到。其实很简单，为了方便，先将所有的数字转换成字符串，想要比较字符串`a`和字符串`b`的大小，就是比较拼接之后的字符串`ab`和字符串`ba`的大小。如果`ab < ba`，则`a<b`。

```python
class Solution(object):
    def largestNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: str
        """
        res = [str(item) for item in nums]
        res = sorted(res,key=cmp_to_key(self.func_sort))
        s = ''.join(res)
        if s.count('0') == len(s):
            return '0'
        return s
    # 自定义比较函数
    def func_sort(self,x,y):
        lenx,leny = len(x),len(y)
        xx = x+y
        yy = y+x
        if xx>yy:
            return -1 # -1表示x在前，y在后
        elif x<y:
            return  1 
        return 0
```

