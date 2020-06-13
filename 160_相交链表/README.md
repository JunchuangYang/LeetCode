# [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

编写一个程序，找到两个单链表相交的起始节点。

如下面的两个链表**：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

在节点 c1 开始相交。

**思路一：首先遍历找到链表A和B的长度，让长的走到短的长度的时候两个链表一起开始走，有相等的结点为相交结点。**

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        A = headA
        B = headB
        lengthA,lengthB = 0,0
        while A:
            lengthA+=1
            A = A.next
        while B:
            lengthB+=1
            B = B.next
        if lengthA>lengthB:
            lengthA,lengthB=lengthB,lengthA
            headA,headB = headB,headA
        while lengthB>lengthA:
            headB = headB.next
            lengthB-=1
        while lengthA>0:
            if headA == headB:
                return headA
            else:
                headA=headA.next
                headB=headB.next
        return None
```

**思路二：[双指针](<https://leetcode-cn.com/problems/intersection-of-two-linked-lists/solution/xiang-jiao-lian-biao-by-leetcode/>)**

- 创建两个指针 pA 和 pB，分别初始化为链表 A 和 B 的头结点。然后让它们向后逐结点遍历。

- 当 pA 到达链表的尾部时，将它重定位到链表 B 的头结点 (你没看错，就是链表 B); 类似的，当 pB 到达链表的尾部时，将它重定位到链表 A 的头结点。
- 若在某一时刻 pA 和 pB 相遇，则 pA/pB 为相交结点。

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        A = headA
        B = headB
        while A!=B:
            A = A.next if A else headB
            B = B.next if B else headA
        return A
```

