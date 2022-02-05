# 


| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 简单 | [链表](#)、[排序](#) | [合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/) | 

## 题目描述

```
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

示例 1：
    输入：l1 = [1,2,4], l2 = [1,3,4]
    输出：[1,1,2,3,4,4]

示例 2：
    输入：l1 = [], l2 = []
    输出：[]

示例 3：
    输入：l1 = [], l2 = [0]
    输出：[0]

提示：
    两个链表的节点数目范围是 [0, 50]
    -100 <= Node.val <= 100
    l1 和 l2 均按 非递减顺序 排列

```
![示例1图例](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

## 解题思路

依次比较两个链表的每个元素的大小，将较小的节点和当前的头节点进行链接。为了更好的处理头节点，我们都会申请一个哑节点dummy作为头节点，最后返回dummy.next即可。

```
if l1.val < l2.val
    cur.next = l1
    cur = cue.next
    l1 = l1.next
else:
    ....
```


## 难点分析
难点1、边界的处理，一定要注意处理只剩下一个链表的时候，直接进行链接即可，不用在进行遍历。

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* head = new ListNode(0);
        ListNode* node = head;
        while (l1 && l2) {
            if (l1->val < l2->val) {
                head->next = l1;
                head = head->next;
                l1 = l1->next;
            }
            else {
                head->next = l2;
                head = head->next;
                l2 = l2->next; 
            }
        }
        if (l1) head->next = l1;
        if (l2) head->next = l2;
        return node->next;
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
    def mergeTwoLists(self, l1, l2):
        """
        :type list1: Optional[ListNode]
        :type list2: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        dummy = ListNode(-1)
        cur = dummy
        while l1 and l2:
            if l1.val < l2.val:
                cur.next = l1
                cur = cur.next
                l1 = l1.next
            else:
                cur.next = l2
                cur = cur.next
                l2 = l2.next
        if l1:
            cur.next = l1
        if l2: 
            cur.next = l2
        return dummy.next
```

## 复杂度
时间复杂度是O(min(n,m))，其中n和m分别是链表1和链表2的长度，最多遍历两个链表的最短的那一个，因为当剩下的长链表的时候，只需要进行链接即可，不用进行遍历。

空间复杂度是O(1)。

## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 