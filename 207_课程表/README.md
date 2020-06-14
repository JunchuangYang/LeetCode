# [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

你这个学期必须选修 numCourse 门课程，记为 0 到 numCourse-1 。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们：[0,1]

给定课程总量以及它们的先决条件，请你判断是否可能完成所有课程的学习？

示例 1:

输入: 2, [[1,0]] 
输出: true
解释: 总共有 2 门课程。学习课程 1 之前，你需要完成课程 0。所以这是可能的。
示例 2:

输入: 2, [[1,0],[0,1]]
输出: false
解释: 总共有 2 门课程。学习课程 1 之前，你需要先完成课程 0；并且学习课程 0 之前，你还应先完成课程 1。这是不可能的。

**拓扑排序**

[题解](https://leetcode-cn.com/problems/course-schedule/solution/course-schedule-tuo-bu-pai-xu-bfsdfsliang-chong-fa/)

```python
class Solution(object):
    def canFinish(self, numCourses, prerequisites):
        """
        :type numCourses: int
        :type prerequisites: List[List[int]]
        :rtype: bool
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
        while len(queue)>0:
            node = queue.pop(0)
            numCourses -= 1
            for item in adjacency[node]:
                indegrees[item] -= 1
                if indegrees[item] == 0:
                    queue.append(item)
        return numCourses==0
```

