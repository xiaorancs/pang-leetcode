# 82. 删除排序链表中的重复元素 II

## 题目先导

> 排序来排序去，数值虽有序，学习无高低。 —— by 胖胖

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | [链表](#) | [删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/) | 

## 题目描述

```
存在一个按升序排列的链表，给你这个链表的头节点 head ，请你删除链表中所有存在数字重复情况的节点，只保留原始链表中 没有重复出现 的数字。

返回同样按升序排列的结果链表。


示例 1：
输入：head = [1,2,3,3,4,4,5]
输出：[1,2,5]
解释：题目要求删除所有重复的元素，由于3和4都出现两次，因此需要全部删除，最后得到1->2->5。

示例 2：
输入：head = [1,1,1,2,3]
输出：[2,3]
解释：1出现了3次，因此全部删除，最后只剩下2->3。 

提示：
    链表中节点数目在范围 [0, 300] 内
    -100 <= Node.val <= 100
    题目数据保证链表已经按升序排列

```

## 解题思路
题目给出的单链表已经是有序的了，我们不用再进行排序，所有相同的元素必然是相邻的。我们知道删除单链表的节点必须知道其前一个节点，例如：`1->1->2->3->3->4`，我们需要删除所有值是1的节点、已经所有值是3的节点。首先来看3对应节点，为了删除所有3，
1. 我们需要保存前一个节点2对应的节点pre。
2. 当遇到第一个节点cur的值是3的时候，我们需要判断是不是cur的下一个是不是3，如果是，我们就要找到第一个不是3的节点，`while (cur->val != 3) cur = cur->next`。
3. 删除3对应的节点，`pre->next = cur`。

从删除3对应的节点的过程，我们必须要知道3的前一个节点，但是1对应的节点有一个是头结点，没有前一个节点怎么办？

对于单链表的删除、插入等相关问题，我们一般会开辟一个新的临时头节点temp_head，让它指向头节点head，这样的话头节点就和其他节点一样，我们可以使用删除3的方式来删除1。

如果遇到当前节点的值不等其下一个节点的值，则直接进行正常遍历即可，但是遍历的时候，需要保存每个节点的前一个节点。

## 难点分析
难点1、题目要求删除所有存在相同值的节点，如果保存前一个节点是一个问题，这里我们反向思考，当遇到每个节点的时候，不是跟其前一个节点比较，而是向后探测直到遇到不相同的节点，进行删除。


难点2、头节点如何处理，这里头节点的处理是个经典的思路，一般情况下都要申请一个临时的头结点指向当前头节点，让头节点和其他节点进行一样的处理。


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
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head || !head->next) {
            return head;
        }
        ListNode* cur = head;
        ListNode* new_head = new ListNode(-1, head);
        ListNode* pre = new_head; 
        while (cur != nullptr && cur->next != nullptr) {
            // 不重复重复，正常更新
            if (cur->val != cur->next->val) {
                pre = cur;
                cur = cur->next;
            } else { // 存在重复，找到第一个不重复的点
                int val = cur->val;
                while (cur && cur->val == val) {
                    cur = cur->next;
                }
                // 更新
                pre->next = cur;
            }
        }
        return new_head->next;
    }
};
```

**Python Code**
```Python

```

## 复杂度


## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 