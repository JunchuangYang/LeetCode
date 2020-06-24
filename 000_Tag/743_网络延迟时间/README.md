# [743. 网络延迟时间](https://leetcode-cn.com/problems/network-delay-time/)

有 N 个网络节点，标记为 1 到 N。

给定一个列表 times，表示信号经过有向边的传递时间。 times[i] = (u, v, w)，其中 u 是源节点，v 是目标节点， w 是一个信号从源节点传递到目标节点的时间。

现在，我们从某个节点 K 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 -1。

![](https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png)

```
输入：times = [[2,1,1],[2,3,1],[3,4,1]], N = 4, K = 2
输出：2
```

注意:

N 的范围在 [1, 100] 之间。
K 的范围在 [1, N] 之间。
times 的长度在 [1, 6000] 之间。
所有的边 times[i] = (u, v, w) 都有 1 <= u, v <= N 且 0 <= w <= 100。

**深搜：记录从源点到达每个节点的最小时间**

```python
class Solution(object):
    def networkDelayTime(self, times, N, K):
        """
        :type times: List[List[int]]
        :type N: int
        :type K: int
        :rtype: int
        """
        dic = defaultdict(list)
        for item in times:
            dic[item[0]].append([item[1],item[2]])
        self.visited = [float('inf') for _ in range(N+1)]
        self.dfs(dic,K,0)
        self.visited[0] = self.visited[K] = -1
        return  -1 if self.visited.count(float('inf'))!=0 else max(self.visited)
    def dfs(self,dic,node,t):
        if self.visited[node]>t:
            self.visited[node] = t
        else:
            return 
        for item in dic[node]:
            self.dfs(dic,item[0],t+item[1])
```

**迪杰斯特拉：记录从源点到终点的最短路径（该算法要求图中不存在负权边）**

```python
class Solution:
    def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:
        dist = [float('inf')]*(N+1) # 从源点到终点的距离
        visisted = [False]*(N+1) # 标记数组是否访问过
        graph = defaultdict(list)
        # 建图
        dist[K]=dist[0]=0
        for item in times:
            graph[item[0]].append([item[1],item[2]])
            if item[0] == K:
                dist[item[1]]=item[2]
        visisted[K]=True
        for _ in range(1,N):
            curr_node = -1
            curr_maxdist = float('inf')
            for i in range(1,N+1):
                if not visisted[i] and dist[i]<curr_maxdist:
                    curr_maxdist = dist[i]
                    curr_node = i
            # 每个节点都已经访问过
            if curr_node==-1:
                break
            visisted[curr_node] = True
            for i,cost in graph[curr_node]:
                if not visisted[i] and dist[i]>(dist[curr_node]+cost):
                    dist[i] = cost+dist[curr_node]
        ans = max(dist)
        return ans if ans<float('inf') else -1
```

