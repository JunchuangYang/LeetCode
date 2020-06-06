# [29. 两数相除](https://leetcode-cn.com/problems/divide-two-integers/)

给定两个整数，被除数 dividend 和除数 divisor。将两数相除，要求不使用乘法、除法和 mod 运算符。

返回被除数 dividend 除以除数 divisor 得到的商。

整数除法的结果应当截去（truncate）其小数部分，例如：truncate(8.345) = 8 以及 truncate(-2.7335) = -2

```python
示例 1:

输入: dividend = 10, divisor = 3
输出: 3
解释: 10/3 = truncate(3.33333..) = truncate(3) = 3
示例 2:

输入: dividend = 7, divisor = -3
输出: -2
解释: 7/-3 = truncate(-2.33333..) = -2
 

提示：

被除数和除数均为 32 位有符号整数。
除数不为 0。
假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−231,  231 − 1]。本题中，如果除法结果溢出，则返回 231 − 1。

```

**[思路](https://blog.csdn.net/qq_17550379/article/details/84863011?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)**

**已给非常容易想到的解题思路就是先分析我们最后的数字是正数还是负数，我们将其全部转化为正数处理，通过`divisor`的不断叠加，直到累加和超过`dividend`，我们记录累加次数。最后根据结果是不是负数，加上负号即可，同时我们要考虑好边界问题。**(超时)

但是dividend很大divisor很小的时候上述做法就超时了，我们只有尝试新的方法。我们可能设置一种增量式的方案，我们使每次增加的divisor扩大两倍。我们的算法思路如下，对于例1

```
3 * 1 < 10
3 * 2 < 10
3 * 4 > 10
```

所以我们现将2加入到结果中，然后计算我们剩余要填充的值10-2*3 = 4。

```
3 * 1 < 4
3 * 2 > 4
```

所以我们再将1加入到结果中，然后计算我们剩余要填充的值4 - 3 = 1。我们此时发现剩余的值1小于我们的divisor=3，所以我们知道我们的结果就是1+2=3。

对于正负数的处理我们还是按照之前的做法。代码如下

```python
class Solution(object):
    def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
        flag = 1
        if (dividend>0 and divisor<0) or (dividend<0 and divisor>0):
            flag = 0
        dividend = abs(dividend)
        divisor = abs(divisor)
        
        if dividend<divisor:
            return 0
        elif divisor == 1:
            # 判断是否超出正边界
            return (dividend-1 if dividend == 2**31 else dividend) if flag else -dividend
        elif dividend == divisor:
            return 1 if flag else -1
        nums = 0
        # 核心代码
        while  dividend>=divisor:
            div,mi = divisor,1
            while div <= dividend:
                div<<=1
                mi<<=1
            nums+=mi>>1
            dividend-=div>>1
        return nums if flag else -nums

```

