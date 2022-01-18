# 83. 删除排序链表中的重复元素

## 题目先导


| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 简单 | [链表](#) | [删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list) | 

## 题目描述

```
存在一个按升序排列的链表，给你这个链表的头节点 head ，请你删除所有重复的元素，使每个元素 只出现一次 。

返回同样按升序排列的结果链表。

示例 1：
输入：head = [1,1,2]
输出：[1,2]
解释：
    origin: 1->1->2; 重复的元素只保留一个
    new: 1->2

示例 2：
输入：head = [1,1,2,3,3]
输出：[1,2,3]
解释：
    origin: 1->1->2->3->3; 重复的元素只保留一个
    new: 1->2->3

提示：
    链表中节点数目在范围 [0, 300] 内
    -100 <= Node.val <= 100
    题目数据保证链表已经按升序排列

```

## 解题思路
由于题目给出的单链表已经有序，因此所有值相等的节点必然都是临近的。因此我们只要比较`cur->next->val = cur->val`，则表示`cur->next`这个节点需要删除，删除的逻辑就是让`cur->next`的前一个节点直接指向下一个节点：`cur->next = cur->next->next`。

其他的情况则正常遍历链表：`cur=cur->next`即可。


## 难点分析

单链表的题目最需要关注的点是：删除某个节点的时候，必须知道其前一个节点，才能进行断链删除。由于这个题目，我们删除的是cur->next的节点，它的前一个节点就是cur，因此没有显式的指定前一个节点。

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
        ListNode* cur = head;
        ListNode* pre = cur;
        while (cur && cur->next) {
            if (cur->val == cur->next->val) {
                cur->next = cur->next->next;
            } else {
                cur = cur->next;
            }
        }
        return head;
    }
};
```

**Python Code**
```Python

```

## 复杂度
由于只遍历了一个链表，因此时间复杂度是`O(n)`，空间复杂度是`O(1)`。

## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 