# [787. K 站中转内最便宜的航班](https://leetcode-cn.com/problems/cheapest-flights-within-k-stops/)

有 n 个城市通过 m 个航班连接。每个航班都从城市 u 开始，以价格 w 抵达 v。

现在给定所有的城市和航班，以及出发城市 src 和目的地 dst，你的任务是找到从 src 到 dst 最多经过 k 站中转的最便宜的价格。 如果没有这样的路线，则输出 -1。

```
示例 1：

输入: 
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 1
输出: 200
解释: 
城市航班图如下

从城市 0 到城市 2 在 1 站中转以内的最便宜价格是 200，如图中红色所示。
```



![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

```
示例 2：

输入: 
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 0
输出: 500
解释: 
城市航班图如下

从城市 0 到城市 2 在 0 站中转以内的最便宜价格是 500，如图中蓝色所示。
```

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

**深搜：1444ms，较慢**

```python
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
        dic = defaultdict(list)
        self.visited = [0]*n
        for edge in flights:
            dic[edge[0]].append([edge[1],edge[2]])
        self.ans = float('inf')
        self.dfs(dic,src,dst,K,-1,0)
        #print(self.visited)
        return  -1 if self.ans == float('inf') else self.ans
    
    def dfs(self,dic,src,dst,K,num,dists):
        if src==dst and num<=K:
            self.ans = min(self.ans,dists)
            return
        if num>K or self.ans<dists: #剪枝，不然超时
            return 
        for item in dic[src]:
            if num<=K and self.visited[src]==0:
                self.visited[src]=1
                self.dfs(dic,item[0],dst,K,num+1,dists+item[1])
                self.visited[src]=0
        return 

```

**广搜:68ms**

```python
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
        dic = defaultdict(list)
        self.visited = [float('inf')]*n
        for edge in flights:
            dic[edge[0]].append([edge[1],edge[2]])
        q = []
        heapq.heappush(q,[-1,src,0])
        while len(q)>0:
            node = heapq.heappop(q)
            if node[0]>K or self.visited[node[1]]<node[2]:
                continue
            self.visited[node[1]] = node[2]
            if node[1] == dst:
                continue
            for item in dic[node[1]]:
                heapq.heappush(q,[node[0]+1,item[0],node[2]+item[1]])
        #print(self.visited)
        return self.visited[dst] if self.visited[dst]<float('inf') else -1
```

