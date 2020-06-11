# [138. 复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。

要求返回这个链表的 深拷贝。 

我们用一个由 n 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 [val, random_index] 表示：

val：一个表示 Node.val 的整数。
random_index：随机指针指向的节点索引（范围从 0 到 n-1）；如果不指向任何节点，则为  null 。



```python
"""
# Definition for a Node.
class Node:
    def __init__(self, x, next=None, random=None):
        self.val = int(x)
        self.next = next
        self.random = random
"""

class Solution(object):
    def copyRandomList(self, head):
        """
        :type head: Node
        :rtype: Node
        """
        if not head:
            return None
        node = head
        # 复制结点:假设原链表为 1->2->3->4,复制后1->1->2->2->3->3->4->4
        while node:
            temp = node.next
            copy_node = Node(node.val,node.next)
            node.next = copy_node
            node = temp
        # 分配random结点:则复制后节点的random节点为 rootnode.random = node.random.next
        node = head
        rootnode  = head.next
        while node :
            temp = node.next.next
            if node.random:
                rootnode.random = node.random.next
            node = temp
            if rootnode.next:
                rootnode = rootnode.next.next
        node = head
        root = rootnode  = head.next
        # 拆分结点：把复制后的结点差分出去
        while node :
            temp=node.next.next
            node.next = temp
            node = temp
            if rootnode.next:
                temp = rootnode.next.next
                rootnode.next = temp
                rootnode = temp
        return root
```



