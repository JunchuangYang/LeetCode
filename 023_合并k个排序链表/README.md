# [23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

```
示例:

输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
```

**归并+递归**（时间复杂度约为O(knlogk)）

1.首先使用归并排序的思想对每个链表进行两两结合

2.使用递归合并两个链表

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        if len(lists) == 0:
            return None
        if len(lists) == 1:
            return lists[0]
        return self.mergesort(lists)

    def mergesort(self,lists):
        if len(lists) == 1:
            return lists[0]
        mid = len(lists)//2
        left = self.mergesort(lists[:mid])
        right = self.mergesort(lists[mid:])
        return self.dfs(left,right)

    def dfs(self,l1,l2):
        if not l1:
            return l2
        
        if not l2:
            return l1
        
        if l1.val<l2.val:
            l1.next = self.dfs(l1.next,l2)
            return l1
        else:
            l2.next = self.dfs(l1,l2.next)
            return l2
```

