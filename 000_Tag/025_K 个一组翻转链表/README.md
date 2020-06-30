# [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

k 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

 

示例：

给你这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1->4->3->5

当 k = 3 时，应当返回: 3->2->1->4->5

 

说明：

你的算法只能使用常数的额外空间。
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

**模拟：注意细节**

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:
        prehead = ListNode(-1)
        prehead.next = head
        pre,node = prehead,prehead
        num = 0
        while node and node.next:
            node = node.next
            num+=1
            if num%k==0:
                pre = self.reverse(pre,k)
                node = pre
        return prehead.next
    
    def reverse(self,pre,k):
        num = 1
        if num==k:
            return None
        # 使用头插法翻转链表，越过第一个节点，后面每遇到一个节点都插到pre之后
        curtail = pre.next
        curnode = pre.next.next
        while num<k and curnode:
            temp = curnode.next
            curtail.next = temp
            curnode.next = pre.next
            pre.next = curnode
            curnode = temp
            num+=1
        # 返回当前翻转的尾部
        return curtail
```

