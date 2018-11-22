### Add Two Numbers

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.  

You may assume the two numbers do not contain any leading zero, except the number 0 itself.  

Example:

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)  
Output: 7 -> 0 -> 8  
Explanation: 342 + 465 = 807.  

题目大意:给你两个链表，将它们相加后输出新的链表即可。只要注意好处理好结尾时的部分就可以.

### C++版

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* tail=new ListNode(0);
        ListNode* ptr=tail;
        int value=0;
        while(l1!=NULL ||l2!=NULL)
        {
           int v1=0;
           if(l1!=NULL)
           {
               v1=l1->val;
               l1=l1->next;
           }
           int v2=0;
           if(l2!=NULL)
           {
               v2=l2->val;
               l2=l2->next;
           }
           int y=ptr->val+v1+v2;
           ptr->val = y%10;
           value = y/10;
           if(l1!=NULL||l2!=NULL||value!=0)
           {
           ptr->next=new ListNode(value);
           ptr = ptr->next;
           }
           
        }
        return tail;
        
    }
};
```