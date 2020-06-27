# [面试题 17.08. 马戏团人塔](https://leetcode-cn.com/problems/circus-tower-lcci/)

有个马戏团正在设计叠罗汉的表演节目，一个人要站在另一人的肩膀上。出于实际和美观的考虑，在上面的人要比下面的人矮一点且轻一点。已知马戏团每个人的身高和体重，请编写代码计算叠罗汉最多能叠几个人。

示例：

输入：height = [65,70,56,75,60,68] weight = [100,150,90,190,95,110]
输出：6
解释：从上往下数，叠罗汉最多能叠 6 层：(56,90), (60,95), (65,100), (68,110), (70,150), (75,190)
提示：

height.length == weight.length <= 10000

**二分求解最长递增子序列**

```python
#先按照height进行从小到大排序，如果存在height[i]==height[j]，那么则按照wight[i]和weight[j]从大到小排序
#这样做是为了保证后面对于w求最长上升子序列时不会对相同高度的元素重复选择
#比如dp=[1,2,3], height[i:i+1]=[3,3],weight[i:i+1]=[5,4]的话，最终dp=[1,2,3,4]
#反之，结果为dp=[1,2,3,4,5]，高度相同的元素被重复选择。
# 链接：https://leetcode-cn.com/problems/circus-tower-lcci/solution/an-zhao-qian-yi-wei-pai-xu-dui-ling-yi-wei-jin-xin/
class Solution:
    def bestSeqAtIndex(self, height: List[int], weight: List[int]) -> int:
        ans = []
        for i in range(len(height)):
            ans.append([height[i],weight[i]])
        ans.sort(key=lambda x:(x[0],-x[1]))#height升序、weight降序
        sequence = []
        res = 0
        for i in range(len(ans)):
            if len(sequence)==0:
                sequence.append(ans[i][1])
            else:
                if sequence[-1]<ans[i][1]:
                    sequence.append(ans[i][1])
                else:
                    l,r=0,len(sequence)-1
                    index=0
                    while l<=r:
                        mid = l+(r-l)//2
                        if sequence[mid]==ans[i][1]:
                            l = mid
                            break
                        elif sequence[mid]>ans[i][1]:
                            r = mid-1
                        else:
                            l = mid+1
                    sequence[l] =ans[i][1]
            #print(sequence)
            res = max(res,len(sequence))
        return res
```

