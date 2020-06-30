# [面试题 02.08. 环路检测](https://leetcode-cn.com/problems/linked-list-cycle-lcci/)

给定一个有环链表，实现一个算法返回环路的开头节点。
有环链表的定义：在链表中某个节点的next元素指向在它前面出现过的节点，则表明该链表存在环路。

**题目的样例给的看不懂，题意就是链表判环，并且找到入环点。**

**快慢指针：**

算法

- 创建指针 fast 和 slow；
- slow 每走一步， fast 走两步；
- 两者相遇时，将 slow 指针指向 head；
- 以同样的速度移动 fast 和 slow，再次相遇的结点即为所求结果。

[链接](https://leetcode-cn.com/problems/linked-list-cycle-lcci/solution/kuai-man-zhi-zhen-python3-java-by-z1m/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        low,fast = head,head
        while fast and fast.next:
            low = low.next
            fast = fast.next.next
            if low==fast:
                low = head
                while low != fast:
                    low = low.next
                    fast = fast.next
                return low
        return None
```

