# [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

​请判断一个链表是否为回文链表。

示例 1:

输入: 1->2
输出: false
示例 2:

输入: 1->2->2->1
输出: true
进阶：
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

**[官解](<https://leetcode-cn.com/problems/palindrome-linked-list/solution/hui-wen-lian-biao-by-leetcode/>)**

[回文链表（1.栈，2.快慢指针+翻转)](<https://leetcode-cn.com/problems/palindrome-linked-list/solution/hui-wen-lian-biao-1zhan-2kuai-man-zhi-zhen-fan-zhu/>)

**递归：不合题意。有O(n)的空间复杂度**

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def isPalindrome(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        # 关键点
        self.prenode = head

        def dfs(node):
            if not node:
                return True
            if not dfs(node.next):
                return False
            if self.prenode.val != node.val:
                return False
            # 关键点
            self.prenode = self.prenode.next
            return True
        return dfs(head)
```

**快慢指针+翻转**

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def isPalindrome(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        slow = fast = head
        pre = ListNode(0) # 翻转 
        while fast and fast.next:
            p = slow
            slow = slow.next
            fast = fast.next.next

            p.next = pre # 翻转
            pre = p
        if fast:# 模拟后，fast不为空，说明链表为奇数
            slow = slow.next
        while slow and pre:
            if slow.val != pre.val:
                return False
            slow = slow.next
            pre = pre.next
        return True
```

