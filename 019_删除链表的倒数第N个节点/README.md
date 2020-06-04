# [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

示例：

给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
说明：

给定的 n 保证是有效的。

进阶：

你能尝试使用一趟扫描实现吗？

**快慢指针，特别判断一下删除头节点的情况。**

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        node1 , node2 = head, head
        m = n
        k = 0
        while m:
            node1 = node1.next
            k+=1
            m-=1
        pre = node2
        while node1:
            k+=1
            node1 = node1.next 
            pre = node2
            node2 = node2.next
        if k == n:
            return head.next
        else:
            pre.next = node2.next
            return head
```

