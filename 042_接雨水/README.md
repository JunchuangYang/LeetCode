# [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

给定 *n* 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 

示例:

输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6

**以前做过一道类似的题，不过是木板没有宽度的。**

**思路：从两边往中间扫描计算雨水量，由于木板有宽度，需要遍历减去木板的宽度。**

```python
class Solution(object):
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        left,right = 0, len(height)-1
        nums = 0
        while left<right:#从两边往中间扫描
            while left<len(height) and height[left]<=0:#找到第一个左边木板长度大于0的
                left += 1
            while right>=0 and height[right]<=0:#找到第一个右边木板长度大于0的
                right -= 1
            if left >= right: # 出界
                break
            h = min(height[left],height[right]) # 找到边界值最小的高度作为雨水的高度
            nums += (right-left-1)*h # 求中间能存多少的雨水
            # 从两边开始减去当前雨水的高度，计算过的不再需要计算
            height[left]-=h 
            height[right]-=h
            l,r = left+1,right-1
            while l <=r:
                # 减去由于木板宽度所多计算的雨水量
                if height[l]>h:# 如果木板高度高于当前雨水高度，减去h雨水高度
                    nums-=h
                elif height[l]>=0:# 否则则减去木板高度
                    nums-=height[l]
                height[l]-=h # 木板的高度减去雨水的高度
                l+=1
        return nums
```

**看了官方题解，发现了双指针方法还可以这样用**

**[使用双指针](<https://leetcode-cn.com/problems/trapping-rain-water/solution/jie-yu-shui-by-leetcode/>)**

从动态编程方法的示意图中我们注意到，只要`right_max` >`left_max`,积水高度将由`left_max`决定。

所以我们可以认为如果一端有更高的条形块（例如右端），积水的高度依赖于当前方向的高度（从左到右）。当我们发现另一侧（右侧）的条形块高度不是最高的，我们则开始从相反的方向遍历（从右到左）。
我们必须在遍历时维护 `left_max` 和` right_max` ，但是我们现在可以使用两个指针交替进行，实现 1 次遍历即可完成

```python
class Solution(object):
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        left,right = 0, len(height)-1
        nums = 0
        left_max = 0 # 左边最高木板
        right_max = 0 # 右边最高木板
        while left< right:
            if height[left] < height[right]: # 左边当前木板小于右边当前木板，积水高度由左边决定
                if height[left] <= left_max: # 左边当前木板小于左边最高木板
                    nums += left_max-height[left]
                else:
                    left_max = height[left]
                left += 1
            else:
                if height[right] <= right_max:# 右边当前木板小于右边最高木板
                    nums += right_max-height[right]
                else:
                    right_max = height[right]
                right -= 1 
        return nums
```

**使用栈**

- 当前木板高度大于栈顶木板高度

   - 出栈一次， 判空。在出栈一次，求第二次出栈和当前木板高度的水容量
- 入栈

```python
class Solution(object):
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        left,right = 0, len(height)
        nums = 0
        r = []
        while left < right:
            while len(r)>0 and height[r[-1]] < height[left]:
                t = r[-1]
                r.pop(-1)
                if len(r)<=0:
                    break
                nums +=  (min(height[left],height[r[-1]]) - height[t]) * (left - r[-1] - 1)
            r.append(left)
            left+=1
            #print(nums)
        return nums
```

