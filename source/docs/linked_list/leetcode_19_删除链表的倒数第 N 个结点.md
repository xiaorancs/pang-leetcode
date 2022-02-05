# 19. 删除链表的倒数第 N 个结点


| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | [链表](#)| [删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/) | 

## 题目描述

```
给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。


示例 1：
    输入：head = [1,2,3,4,5], n = 2
    输出：[1,2,3,5]

示例 2：
    输入：head = [1], n = 1
    输出：[]

示例 3：
    输入：head = [1,2], n = 1
    输出：[1]

提示：
    链表中结点的数目为 sz
    1 <= sz <= 30
    0 <= Node.val <= 100
    1 <= n <= sz

进阶：你能尝试使用一趟扫描实现吗？

```
![示例1图示](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)


## 解题思路
题目的目标是删除倒数第n个节点，我们知道对于单链表的删除，为了删除倒数第n个节点，需要找到倒数第n-1个节点。

为了找到倒数第n个节点，我们不放倒着看，如果有一个指针fast到了链表的尾部，我们假设slow指针在倒数第n个位置。我们将fast和slow都一直向前，知道slow到达链表头部，此时fast指针刚好是倒数第n个。

因此对于本题目，我们准备两个指针，fast和slow，fast先走n步之后，slow在从头开始和fast一起走，直到fast走到尾部，slow刚好达到倒数第n个节点，我们只需要一直记录slow前面的一个节点pre就可以完成删除。pre.next = slow.next。

## 难点分析

难点1、需要注意如果删除的是第一个节点，此时要删除的节点是头节点，需要单独进行删除。head = head.next

## 代码
- 代码支持：C++、Python

**C++ Code**
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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        // 先走n+1，后跟进，如果先走到达null，后面指针指向的前一个
        int i = 0;
        ListNode* fast = head;
        ListNode* pre = NULL;
        ListNode* slow = head;
        while (i < n && fast != NULL) {
            fast = fast->next;
            i ++;
        }
        while (fast != NULL) {
            pre = slow;
            fast = fast->next;
            slow = slow->next;
        }
        if (pre == NULL) { // 删除头结点
            head = head->next;
        }
        else { // remove
            pre->next = slow->next;
        }
        return head;
    }
};
```

**Python Code**
```Python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        # 为了删除倒数第n个节点，我们需要找到倒数第n-1个节点，
        # 使用两个指针，第一个fast先走n次，第二个slow再从头开始走，
        # 当fast走到最后一个，slow刚好走到第n个
        i = 0
        fast = head
        pre = None
        slow = head
        # fast 先走n
        while i < n and fast != None:
            fast = fast.next
            i += 1

        while fast != None:
            pre = slow
            slow = slow.next
            fast = fast.next
        
        # 判断删除是不是第一个节点
        if pre is None:
            head = head.next
        else:
            pre.next = slow.next

        return head
```

## 复杂度
由于只遍历了依次链表，因此时间复杂度是O(n)，空间复杂书是O(1)。

## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 