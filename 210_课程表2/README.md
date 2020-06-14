# [210. 课程表 II](https://leetcode-cn.com/problems/course-schedule-ii/)


现在你总共有 *n* 门课需要选，记为 `0` 到 `n-1`。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: `[0,1]`

给定课程总量以及它们的先决条件，返回你为了学完所有课程所安排的学习顺序。

可能会有多个正确的顺序，你只要返回一种就可以了。如果不可能完成所有课程，返回一个空数组。

**示例 1:**

```
输入: 2, [[1,0]] 
输出: [0,1]
解释: 总共有 2 门课程。要学习课程 1，你需要先完成课程 0。因此，正确的课程顺序为 [0,1] 。
```

**示例 2:**

```
输入: 4, [[1,0],[2,0],[3,1],[3,2]]
输出: [0,1,2,3] or [0,2,1,3]
解释: 总共有 4 门课程。要学习课程 3，你应该先完成课程 1 和课程 2。并且课程 1 和课程 2 都应该排在课程 0 之后。
     因此，一个正确的课程顺序是 [0,1,2,3] 。另一个正确的排序是 [0,2,1,3] 。
```



**拓扑排序，记录路径**

```python
class Solution(object):
    def findOrder(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: List[int]
        """
        # 拓扑排序：检测有向图是否有环
        # 入度表
        indegrees = [0 for _ in range(numCourses)]
        # 邻接表
        adjacency = [[] for _ in range(numCourses)]
        queue = []
        for item in prerequisites:
            indegrees[item[0]] += 1
            adjacency[item[1]].append(item[0])
        for i in range(numCourses):
            if indegrees[i]==0:
                queue.append(i)
        res = []
        while len(queue)>0:
            node = queue.pop(0)
            res.append(node)
            numCourses -= 1
            for item in adjacency[node]:
                indegrees[item] -= 1
                if indegrees[item] == 0:
                    queue.append(item)
        return res if numCourses==0 else []
```

