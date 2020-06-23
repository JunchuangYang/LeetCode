# [692. 前K个高频单词](https://leetcode-cn.com/problems/top-k-frequent-words/)

给一非空的单词列表，返回前 k 个出现次数最多的单词。

返回的答案应该按单词出现频率由高到低排序。如果不同的单词有相同出现频率，按字母顺序排序。

示例 1：

输入: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
输出: ["i", "love"]
解析: "i" 和 "love" 为出现次数最多的两个单词，均为2次。
​    注意，按字母顺序 "i" 在 "love" 之前。


示例 2：

输入: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
输出: ["the", "is", "sunny", "day"]
解析: "the", "is", "sunny" 和 "day" 是出现次数最多的四个单词，
​    出现次数依次为 4, 3, 2 和 1 次。


注意：

假定 k 总为有效值， 1 ≤ k ≤ 集合元素数。
输入的单词均由小写字母组成。


扩展练习：

尝试以 O(n log k) 时间复杂度和 O(n) 空间复杂度解决。

**小顶堆：注意频率相同时按字典序对堆进行调整，非常重要。**

```python
class Solution:
    def topKFrequent(self, words: List[str], k: int) -> List[str]:
        dic = defaultdict(list)
        for item in words:
            if dic[item]:
                dic[item][0]+=1
            else:
                dic[item] = [1,item]
        heap = []
        for key in dic.keys():
            if len(heap)<k:
                heap.append(dic[key])
                n = len(heap)-1
                while n>0 and (heap[(n-1)//2][0] > heap[n][0] or (heap[(n-1)//2][0] == heap[n][0] and
                heap[(n-1)//2][1]<heap[n][1])):
                    heap[(n-1)//2],heap[n] = heap[n], heap[(n-1)//2]
                    n = (n-1)//2
            else:
                if heap[0][0]<dic[key][0]:
                    heap[0] = dic[key]
                    self.adjustheap(heap,0)
                # 次数相同时字母顺序在前的要进堆
                elif heap[0][0] == dic[key][0] and heap[0][1]>dic[key][1]:
                    heap[0] = dic[key]
                    self.adjustheap(heap,0)
                #print (heap)                    
        heap.sort(key = lambda x:(-x[0],x[1]))
        #print(heap)
        res = []
        for item in heap:
            res.append(item[1])
        return res
    def adjustheap(self,heap,root):
        n = len(heap)-1
        while True:
            child = root*2+1
            if child>n:
                break
            if child+1<=n and heap[child][0]>heap[child+1][0]:
                child = child+1
            elif child+1<=n and heap[child][0]==heap[child+1][0] and heap[child][1]<heap[child+1][1]:
                child = child+1
            if heap[root][0] > heap[child][0]:
                heap[root],heap[child] = heap[child],heap[root]
                root=child
            elif heap[root][0] == heap[child][0] and heap[root][1] < heap[child][1]:
                heap[root],heap[child] = heap[child],heap[root]
                root=child
            else:
                break
```

