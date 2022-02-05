# 25. K 个一组翻转链表

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 困难 | [链表](#) | [K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/) | 

## 题目描述

```
给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

k 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

进阶：

你可以设计一个只使用常数额外空间的算法来解决此问题吗？
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

示例 1：
    输入：head = [1,2,3,4,5], k = 2
    输出：[2,1,4,3,5]
```
![示例1](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)
```
示例 2：
    输入：head = [1,2,3,4,5], k = 3
    输出：[3,2,1,4,5]
```
![示例2](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)
```
示例 3：
    输入：head = [1,2,3,4,5], k = 1
    输出：[1,2,3,4,5]


示例 4：
    输入：head = [1], k = 1
    输出：[1]

提示：
    列表中节点的数量在范围 sz 内
    1 <= sz <= 5000
    0 <= Node.val <= 1000
    1 <= k <= sz
```

## 解题思路
这个题目可以拆分成两个子题目，第一个是每次找到k个节点，第二次是对这k个节点进行翻转，之后返回新的头尾节点，再进行链接上。

子题目1：依次找到k个节点，使用[start_node, end_node]表示，每次从head头节点开始，向后走k个节点，生成待翻转的链表，v表记录下一个节点。


子题目2：对[start_node, end_node]使用头插法进行翻转，并返回新的头尾节点[new_start_node, new_end_node]；将这k个节点和之前的节点链接上。


## 难点分析

难点1、翻转的时候要返回新的头尾节点，进行链接。


## 代码
- 代码支持：C++、Python

**C++ Code**
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    pair<ListNode*, ListNode*> reverse(ListNode* head, ListNode* tail) {
        ListNode* prev = nullptr;
        ListNode* p = head;
        while (prev != tail) {
            ListNode* next = p->next;
            p->next = prev;
            prev = p;
            p = next;
        }
        return {tail, head};
    }

    ListNode* reverseKGroup(ListNode* head, int k) {
        // 每k个翻转
        ListNode* hair = new ListNode(-1);
        hair->next = head;
        ListNode* pre = hair;

        while (head) {
            ListNode* tail = pre;
            for (int i = 0; i < k; i++) {
                tail = tail->next;
                if (!tail) { // 不满足k
                    return hair->next;
                }
            }
            ListNode* next = tail->next;
            tie(head, tail) = reverse(head, tail);
            pre->next = head;
            tail->next = next;
            pre = tail;
            head = next;
        }
        return hair->next;
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
    
    def reverse(self, start, end):
        # 头插法
        prev = None
        cur = start
        while cur != end:
            next = cur.next
            cur.next = prev
            prev = cur
            cur = next
        # 处理最后一个
        next = cur.next
        cur.next = prev
        prev = cur
        cur = next

        return end, start

    
    def reverseKGroup(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        # 每次找到k个链表，进行翻转，之后返回新的头尾节点
        dummy = ListNode(-1, head)
        cur = head
        prev = dummy

        while head:
            end = prev
            for i in range(k):
                end = end.next
                if end is None:
                    return dummy.next
            # print(head.val, end.val)
            # 记录下一个节点
            next = end.next
            head, end = self.reverse(head, end)
            # print(head.val, end.val)

            prev.next = head
            end.next = next
            # 更新当前链表的最后一个节点
            prev = end
            # 记录新的节点继续访问
            head = next
        return dummy.next
```

## 复杂度

遍历一次链表的时间复杂度是O(n)，每次翻转k长度的链表的时间复杂度是O(k)，最多翻转n次，时间复杂度是O(n)，总的时间复杂度是O(2n)。

由于只申请了常数个节点变量，因此空间复杂度是O(1)。

## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 