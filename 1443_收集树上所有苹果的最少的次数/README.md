# [1443. 收集树上所有苹果的最少时间](https://leetcode-cn.com/problems/minimum-time-to-collect-all-apples-in-a-tree/)

给你一棵有 n 个节点的无向树，节点编号为 0 到 n-1 ，它们中有一些节点有苹果。通过树上的一条边，需要花费 1 秒钟。你从 节点 0 出发，请你返回最少需要多少秒，可以收集到所有苹果，并回到节点 0 。

无向树的边由 edges 给出，其中 edges[i] = [fromi, toi] ，表示有一条边连接 from 和 toi 。除此以外，还有一个布尔数组 hasApple ，其中 hasApple[i] = true 代表节点 i 有一个苹果，否则，节点 i 没有苹果。

示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/10/min_time_collect_apple_1.png)

输入：n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,true,true,false]
输出：8 
解释：上图展示了给定的树，其中红色节点表示有苹果。一个能收集到所有苹果的最优方案由绿色箭头表示。
示例 2：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/05/10/min_time_collect_apple_2.png)

输入：n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,false,true,false]
输出：6
解释：上图展示了给定的树，其中红色节点表示有苹果。一个能收集到所有苹果的最优方案由绿色箭头表示。

示例 3：

输入：n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,false,false,false,false,false]
输出：0


提示：

- 1 <= n <= 10^5
- edges.length == n-1
- edges[i].length == 2
- 0 <= fromi, toi <= n-1
- fromi < toi
- hasApple.length == n

**深搜：从根节点往下搜索，遇到苹果+1，记录路径；直到苹果个数达到总数时停止搜索。**

**超时:从上往下搜索**

```python
class Solution:
    def minTime(self, n: int, edges: List[List[int]], hasApple: List[bool]) -> int:
        self.nums=0
        for item in hasApple:
            if item:
                self.nums+=1
        self.dic=defaultdict(list)
        for item in edges:
            self.dic[item[0]].append(item[1])
            self.dic[item[1]].append(item[0])
        self.ans = float('inf')
        self.apples=0
        if hasApple[0]:
            self.apples+=1
        self.dfs(0,edges,hasApple,[0])
        return self.ans*2
    def dfs(self,node,edges,hasApple,temp):
        if self.apples==self.nums:
            #print(temp)
            self.ans = min(self.ans,len(temp)-1)
            return 
        for i in range(len(self.dic[node])):
            if self.dic[node][i] not in temp:
                temp.append(self.dic[node][i])
                if hasApple[self.dic[node][i]]:
                    self.apples+=1
                self.dfs(self.dic[node][i],edges,hasApple,temp)
                if temp[-1]==self.dic[node][i] and not hasApple[self.dic[node][i]]:
                    temp.pop()
        return
```

**从下往上搜索：搜索有苹果的结点，从苹果处到根节点的路径记录下来；其他有苹果的结点只要搜索到记录路径中的结点。**

**还是超时**

```python
class Solution:
    def minTime(self, n: int, edges: List[List[int]], hasApple: List[bool]) -> int:
        self.dic=defaultdict(list)
        for item in edges:
            self.dic[item[0]].append(item[1])
            self.dic[item[1]].append(item[0])
        self.temp = [] # 路径
        for i in range(len(hasApple)):
            if hasApple[i] and i not in self.temp:
                self.temp.append(i)
                self.dfs(i)
        if len(self.temp)==0:
            return 0
        #print(self.temp)
        return (len(self.temp)-1)*2
    def dfs(self,node):
        if node==0:
            return True
        for i in range(len(self.dic[node])):
            if self.dic[node][i] not in self.temp:
                self.temp.append(self.dic[node][i])
                if self.dfs(self.dic[node][i]):
                    return True
                else:
                    self.temp.pop()
            elif self.dic[node][i]==0 or 0 in self.temp:
                return True
        return False
```

**我还是菜**



题解：https://blog.csdn.net/JR_Chan/article/details/106152724

```python
class Solution:
    def minTime(self, n: int, edges: List[List[int]], hasApple: List[bool]) -> int:
        self.dic=defaultdict(list)
        self.flag = [0 for _ in range(n)]
        for item in edges:
            self.dic[item[0]].append(item[1])
            self.dic[item[1]].append(item[0])
        result = self.dfs(0,hasApple)
        return result if result == 0 else result-2
    def dfs(self,node,hasApple):
        self.flag[node] = 1
        res = 0
        for i in range(len(self.dic[node])):
            if self.flag[self.dic[node][i]]==0:
                res+=self.dfs(self.dic[node][i],hasApple)
        if res!=0:
            return res+2
        if hasApple[node]:
            return 2
        return 0

```

