# 24. 两两交换链表中的节点

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | [链表](#) | [两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/) | 

## 题目描述

```
给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

示例 1：
    输入：head = [1,2,3,4]
    输出：[2,1,4,3]

示例 2：
    输入：head = []
    输出：[]

示例 3：
    输入：head = [1]
    输出：[1]


提示：
    链表中节点的数目在范围 [0, 100] 内
    0 <= Node.val <= 100
```
![示例1图示](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

## 解题思路

思路比较清晰，每次判断当前节点cur和当前节点的下一个节点cur.next，如果两个节点都存在，则交换这两个节点即可，但是需要注意的时候，交换节点之前，需要记录cur.next.next节点，作为下一个cur节点解析迭代。

## 难点分析

难点1、交换节点的时候，需要保存下一个节点作为迭代进行记录。


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
    ListNode* swapPairs(ListNode* head) {
        ListNode* node = new ListNode(0);
        node->next = head;
        ListNode* pre = node;
        ListNode* cur = head;

        while (cur && cur->next) {
            pre->next = cur->next;
            ListNode* tmp = cur->next->next;
            cur->next->next = cur;
            cur->next = tmp;
            pre = cur;
            cur = tmp;
        }
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
    def swapPairs(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        dummy = ListNode(-1, head)
        cur = head
        pre = dummy

        while cur and cur.next:
            # 先记录下一个
            tmp = cur.next.next
            # 交换cur和cur.next
            pre.next = cur.next
            cur.next.next = cur
            # 连接断链，继续迭代
            cur.next = tmp
            # 更新每个节点的前一个节点
            pre = cur
            cur = tmp
        return dummy.next
```

## 复杂度

由于只遍历了一次链表，因此时间复杂度是O(n)；只申请了有限个空间节点，因此空间复杂度是O(1)。


## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 