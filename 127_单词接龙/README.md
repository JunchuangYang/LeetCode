# [127. 单词接龙](https://leetcode-cn.com/problems/word-ladder/)

给定两个单词（beginWord 和 endWord）和一个字典，找到从 beginWord 到 endWord 的最短转换序列的长度。转换需遵循如下规则：

每次转换只能改变一个字母。
转换过程中的中间单词必须是字典中的单词。
说明:

如果不存在这样的转换序列，返回 0。
所有单词具有相同的长度。
所有单词只由小写字母组成。
字典中不存在重复的单词。
你可以假设 beginWord 和 endWord 是非空的，且二者不相同。

```
示例 1:

输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出: 5

解释: 一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog",
     返回它的长度 5。
示例 2:

输入:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

输出: 0

解释: endWord "cog" 不在字典中，所以无法进行转换。
```

[官解](<https://leetcode-cn.com/problems/word-ladder/solution/dan-ci-jie-long-by-leetcode/>)

**广搜：BFS,2688ms**

```python
from collections import defaultdict
class Solution(object):
    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: int
        """
        if endWord not in wordList:
            return 0
        # 关联字典
        dic = defaultdict(list)
        for i in range(len(beginWord)):
            dic[beginWord[0:i] + '*' + beginWord[i+1:]].append(beginWord)
        for word in wordList:
            for i in range(len(word)):
                dic[word[0:i] + '*' + word[i+1:]].append(word)

        # 广搜
        q = [[beginWord,1]]
        visited = [beginWord]
        while len(q)>0:
            l = q.pop(0)
            word,n = l[0],l[1]
            for i in range(len(word)):
                for item in dic[word[0:i] + '*' + word[i+1:]]:
                    if item == endWord:
                        return n+1
                    if item not in visited:
                        q.append([item,n+1])
                        visited.append(item)
        return 0
        
            
```

**双向BFS，116ms，比上面快了10倍还多。**

从前后同时开始找，当一个单词在另一个方向的visited数组中出现的时候搜索结束。

```python
from collections import defaultdict
class Solution(object):
    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: int
        """
        if endWord not in wordList:
            return 0
        # 关联字典
        self.dic = defaultdict(list)
        for i in range(len(beginWord)):
            self.dic[beginWord[0:i] + '*' + beginWord[i+1:]].append(beginWord)
        for word in wordList:
            for i in range(len(word)):
                self.dic[word[0:i] + '*' + word[i+1:]].append(word)
        # 广搜
        q_begin = [[beginWord,1]]
        visited_begin = {beginWord:1}
        q_end = [[endWord,1]]
        visited_end = {endWord:1}
        while len(q_begin)>0 and len(q_end)>0:
            ans = self.BFS(q_begin,visited_begin,visited_end)
            if ans:
                return ans
            ans = self.BFS(q_end,visited_end,visited_begin)
            if ans:
                return ans
        return 0
    def BFS(self,q,visited,othervisited):
        l = q.pop(0)
        word,n = l[0],l[1]
        for i in range(len(word)):
            for item in self.dic[word[0:i] + '*' + word[i+1:]]:
                if item in othervisited:
                    return n + othervisited[item]
                if item not in visited.keys():
                    visited[item] = n+1
                    q.append([item,n+1])
        return None
```

