# [1171. 从链表中删去总和值为零的连续节点](https://leetcode-cn.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/)

给你一个链表的头节点 head，请你编写代码，反复删去链表中由 总和 值为 0 的连续节点组成的序列，直到不存在这样的序列为止。

删除完毕后，请你返回最终结果链表的头节点。

 

你可以返回任何满足题目要求的答案。

（注意，下面示例中的所有序列，都是对 ListNode 对象序列化的表示。）

```
示例 1：

输入：head = [1,2,-3,3,1]
输出：[3,1]
提示：答案 [1,2,1] 也是正确的。
示例 2：

输入：head = [1,2,3,-3,4]
输出：[1,2,4]
示例 3：

输入：head = [1,2,3,-3,-2]
输出：[1]
 

提示：

给你的链表中可能有 1 到 1000 个节点。
对于链表中的每个节点，节点的值：-1000 <= node.val <= 1000.
```

思路：

**我们可以考虑如果给的入参不是链表是数组的话，只需要求出前缀和，对于前缀和相同的项，那他们中间的部分即是可以消除掉的，比如以 [1, 2, 3, -3, 4] 为例，其前缀和数组为 [1, 3, 6, 3, 7] ，我们发现有两项均为 3，则 6 和 第二个 3 所对应的原数组中的数字是可以消掉的。换成链表其实也是一样的思路，把第一个 3 的 next 指向第二个 3 的 next 即可.**
[链接](https://leetcode-cn.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/solution/c-jian-ji-dai-si-lu-by-philhsu/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def removeZeroSumSublists(self, head: ListNode) -> ListNode:
        # 前缀和：前缀和相同的数字之间的结点可以删除
        dic = {}
        prehead = ListNode(0)
        prehead.next = head
        nums = 0
        node = prehead
        while node:
            nums+=node.val
            if dic.get(nums,0)==0:
                dic[nums] = node
            else:
                # 删除前缀和相同的之间的结点的前缀和
                temp = dic[nums].next
                n = nums
                while temp!=node:
                    n += temp.val
                    temp = temp.next
                    del dic[n]
                dic[nums].next = node.next
            node = node.next
        return prehead.next 
```

