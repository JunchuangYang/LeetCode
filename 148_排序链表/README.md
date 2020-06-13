# [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

在 *O*(*n* log *n*) 时间复杂度和常数级空间复杂度下，对链表进行排序。

```
示例 1:

输入: 4->2->1->3
输出: 1->2->3->4

示例 2:

输入: -1->5->3->4->0
输出: -1->0->3->4->5

```

**归并排序,自底向上**

题解

<https://leetcode-cn.com/problems/sort-list/solution/sort-list-gui-bing-pai-xu-lian-biao-by-jyd/>

<https://leetcode-cn.com/problems/sort-list/solution/kong-jian-wei-o1nlognde-fu-za-du-by-dukeenglish/>



```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def sortList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        # 特判
        if not head or not head.next:
            return head
        # 获取链表长度
        length = 0
        node = head
        while node:
            length += 1
            node = node.next
        step = 1 # 从1开始,依次进行链表结点合并（1->2->4）
        res = ListNode(0)
        res.next = head
        while step<length:
            pre = temp = res
            temp = temp.next
            while temp:
                odd_length , even_length = 0,0
                odd_node = temp
                while odd_length < step and temp:
                    temp = temp.next
                    odd_length+=1
                even_node = temp
                while even_length < step and temp:
                    temp = temp.next
                    even_length += 1
                # 开始合并
                while odd_node and even_node and odd_length>0 and even_length>0:
                    if  odd_node.val>even_node.val:
                        pre.next = even_node
                        even_node = even_node.next
                        pre = pre.next
                        even_length -= 1
                    else:
                        pre.next = odd_node
                        odd_node = odd_node.next
                        pre = pre.next
                        odd_length -= 1
                while odd_length>0:
                    pre.next = odd_node
                    odd_node = odd_node.next
                    pre = pre.next
                    odd_length -= 1 
                while even_length>0:
                    pre.next = even_node
                    even_node = even_node.next
                    pre = pre.next
                    even_length -=1 
            pre.next = None
            step *= 2
        return res.next
```

