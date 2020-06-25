# [864. 获取所有钥匙的最短路径](https://leetcode-cn.com/problems/shortest-path-to-get-all-keys/)

给定一个二维网格 grid。 "." 代表一个空房间， "#" 代表一堵墙， "@" 是起点，（"a", "b", ...）代表钥匙，（"A", "B", ...）代表锁。

我们从起点开始出发，一次移动是指向四个基本方向之一行走一个单位空间。我们不能在网格外面行走，也无法穿过一堵墙。如果途经一个钥匙，我们就把它捡起来。除非我们手里有对应的钥匙，否则无法通过锁。

假设 K 为钥匙/锁的个数，且满足 1 <= K <= 6，字母表中的前 K 个字母在网格中都有自己对应的一个小写和一个大写字母。换言之，每个锁有唯一对应的钥匙，每个钥匙也有唯一对应的锁。另外，代表钥匙和锁的字母互为大小写并按字母顺序排列。

返回获取所有钥匙所需要的移动的最少次数。如果无法获取所有钥匙，返回 -1 。

```python
示例 1：

输入：["@.a.#","###.#","b.A.B"]
输出：8
示例 2：

输入：["@..aA","..B#.","....b"]
输出：6

提示：
1 <= grid.length <= 30
1 <= grid[0].length <= 30
grid[i][j] 只含有 '.', '#', '@', 'a'-'f' 以及 'A'-'F'
钥匙的数目范围是 [1, 6]，每个钥匙都对应一个不同的字母，正好打开一个对应的锁。

```

**BFS:难点在于走过的点还可以在遍历**

visited不仅要存当前点的坐标，也要记录当前获得的钥匙情况，因为如果绕远获得了钥匙，那么是可以重走已经走过的路的。

**看到题解中还有人用到状态压缩的。**

运行时间比较慢。

```python
class Solution:
    def shortestPathAllKeys(self, grid: List[str]) -> int:
        key_num = 0
        startx,starty = 0,0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if 'a'<=grid[i][j]<='z':
                    key_num+=1
                elif grid[i][j] == '@':
                    startx,starty=i,j
        #BFS
        heap= [[0,startx,starty,[]]]
        visited = defaultdict(int)
        visited[tuple([startx,starty,tuple([])])]=0
        heapq.heapify(heap)
        ans = -1
        while len(heap)>0:
            step,x,y,key = heapq.heappop(heap)
            #print(key)
            if len(key) == key_num:
                return step
            for i,j in [[x+1,y],[x-1,y],[x,y+1],[x,y-1]]:
                keys = key[:]
                flag = tuple([i,j,tuple(keys)]) # 标记路径和到该路径获得的钥匙的状态
                if i>=0 and i<len(grid) and j>=0 and j<len(grid[0]) and grid[i][j]!='#' and (flag not in visited or visited[flag]>step+1):
                    if 'a'<=grid[i][j]<='z':
                        if grid[i][j] not in keys:
                            keys.append(grid[i][j]) 
                        visited[flag] = step+1
                        heapq.heappush(heap,[step+1,i,j,keys])
                    elif 'A'<=grid[i][j]<='Z':
                        if grid[i][j].lower() in keys:
                            visited[flag] = step+1
                            heapq.heappush(heap,[step+1,i,j,keys])
                    else:
                        heapq.heappush(heap,[step+1,i,j,keys])
                        visited[flag] = step+1
        return -1
```

