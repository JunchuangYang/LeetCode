# [877. 石子游戏](https://leetcode-cn.com/problems/stone-game/)

亚历克斯和李用几堆石子在做游戏。偶数堆石子排成一行，每堆都有正整数颗石子 piles[i] 。

游戏以谁手中的石子最多来决出胜负。石子的总数是奇数，所以没有平局。

亚历克斯和李轮流进行，亚历克斯先开始。 每回合，玩家从行的开始或结束处取走整堆石头。 这种情况一直持续到没有更多的石子堆为止，此时手中石子最多的玩家获胜。

假设亚历克斯和李都发挥出最佳水平，当亚历克斯赢得比赛时返回 true ，当李赢得比赛时返回 false 。

```
示例：

输入：[5,3,4,5]
输出：true
解释：
亚历克斯先开始，只能拿前 5 颗或后 5 颗石子 。
假设他取了前 5 颗，这一行就变成了 [3,4,5] 。
如果李拿走前 3 颗，那么剩下的是 [4,5]，亚历克斯拿走后 5 颗赢得 10 分。
如果李拿走后 5 颗，那么剩下的是 [3,4]，亚历克斯拿走后 4 颗赢得 9 分。
这表明，取前 5 颗石子对亚历克斯来说是一个胜利的举动，所以我们返回 true 。
 

提示：

2 <= piles.length <= 500
piles.length 是偶数。
1 <= piles[i] <= 500
sum(piles) 是奇数。
```

**动态规划：第一次靠自己写出的动归题，开心。**

```python
class Solution:
    def stoneGame(self, piles: List[int]) -> bool:
        # dp[i][j][k]:表示在i,j这段数字内拿的是哪个数字，k=0拿i，k=1拿j
        # dp[i][j][0] = max(dp[i-2][j][0] , dp[i][j+2][1])+piles[i]
        # dp[i][j][1] = max(dp[i-2][j][0] , dp[i][j+2][1])+piles[j]
        dp = [[[0]*2 for i in range(len(piles)+1)] for _ in range(len(piles)+1)]
        res = 0
        sumnums = 0
        for i in range(len(piles)):
            sumnums += piles[i]
        for i in range(0,len(piles)):
            for j in range(len(piles)-1,i-1,-1):
                for k in range(2):
                    index = i if k==0 else j
                    if j+2<len(piles) and i-2>=0:
                        dp[i][j][k] = max(dp[i-2][j][0],dp[i][j+2][1])+piles[index]
                    elif i-2>=0:
                        dp[i][j][k] = dp[i-2][j][0]+piles[index]
                    elif j+2<len(piles):
                        dp[i][j][k] = dp[i][j+2][1]+piles[index]
                    else:
                        dp[i][j][k] = piles[index]
                    res = max(dp[i][j][k],res)
        print(res)
        if sumnums-res<res:
            return True
        return False
```

----

---

---

华丽的分割线

## **惊雷**

其实只需要`return True`,什么dp什么搜索统统一遍去。

**开心个屁！**

**思路和算法**

显然，亚历克斯总是赢得 2 堆时的游戏。 通过一些努力，我们可以获知她总是赢得 4 堆时的游戏。

如果亚历克斯最初获得第一堆，她总是可以拿第三堆。 如果她最初取到第四堆，她总是可以取第二堆。第一 + 第三，第二 + 第四 中的至少一组是更大的，所以她总能获胜。

我们可以将这个想法扩展到 N 堆的情况下。设第一、第三、第五、第七桩是白色的，第二、第四、第六、第八桩是黑色的。 亚历克斯总是可以拿到所有白色桩或所有黑色桩，其中一种颜色具有的石头数量必定大于另一种颜色的。

因此，亚历克斯总能赢得比赛。

```python
class Solution:
    def stoneGame(self, piles: List[int]) -> bool:
        return True
```

