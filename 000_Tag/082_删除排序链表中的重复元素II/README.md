# [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 没有重复出现 的数字。

示例 1:

输入: 1->2->3->3->4->4->5
输出: 1->2->5
示例 2:

输入: 1->1->1->2->3
输出: 2->3

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        prehead = ListNode(0)
        prehead.next = head
        temp = prehead
        node = head 
        while node:
            flag = 0
            while node.next and node.val == node.next.val:
                flag = 1
                node = node.next
            if flag:
                temp.next = node.next
            else:
                temp = node
            node = node.next
        return prehead.next
```

