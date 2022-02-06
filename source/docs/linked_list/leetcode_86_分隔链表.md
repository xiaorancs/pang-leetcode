# 86. 分隔链表


| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | [链表](#) | [分隔链表](https://leetcode-cn.com/problems/partition-list/) | 

## 题目描述

<p>给你一个链表的头节点 <code>head</code> 和一个特定值<em> </em><code>x</code> ，请你对链表进行分隔，使得所有 <strong>小于</strong> <code>x</code> 的节点都出现在 <strong>大于或等于</strong> <code>x</code> 的节点之前。</p>

<p>你应当 <strong>保留</strong> 两个分区中每个节点的初始相对位置。</p>

<p><strong>示例 1：</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/04/partition.jpg" style="width: 662px; height: 222px;" />
<pre>
<strong>输入：</strong>head = [1,4,3,2,5,2], x = 3
<strong>输出</strong>：[1,2,2,4,3,5]
</pre>

<p><strong>示例 2：</strong></p>

<pre>
<strong>输入：</strong>head = [2,1], x = 2
<strong>输出</strong>：[1,2]
</pre>


<p><strong>提示：</strong></p>

<ul>
	<li>链表中节点的数目在范围 <code>[0, 200]</code> 内</li>
	<li><code>-100 <= Node.val <= 100</code></li>
	<li><code>-200 <= x <= 200</code></li>
</ul>


## 解题思路

题目要求小于x的节点全部移动到左边，大于等于x的节点移动到x的右边。

思路1、我们可以参考快速排序的一次遍历的思路，遇到当前节点cur，如果cur.val < x，则将这个cur节点拆入都链表头部，否则，将这个节点移动到链表的尾部。这种方法不能保证修改后的链表维持原来的相对序关系。

思路2、我们可以将小于x的节点单独组成一个链表，大于等于x的节点组成另一个链表。我们初始化两个头节点，分别是small和large分别记录小于x和不小于x的链表。最后将两个链表链接，即small链表的尾部链接到large链表的头部。

## 难点分析

难点1、如何保证修改之后，不改变原始节点的相对位置。

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
    ListNode* partition(ListNode* head, int x) {
        // 方案2. 初始化两个链表，一个保存小于x的节点，一个链接大于等于x的节点，最后再将两个链表链接起来。
        ListNode* small = new ListNode(-1);
        ListNode* large = new ListNode(-1);

        ListNode* cur_small = small;
        ListNode* cur_large = large;

        ListNode* cur = head;
        while (cur != nullptr) {
            ListNode* temp = cur->next;
            if (cur->val < x) {
                cur_small->next = cur;
                cur->next = nullptr;
                cur_small = cur_small->next;
            } else {
                cur_large->next = cur;
                cur->next = nullptr;
                cur_large = cur_large->next;                
            }
            cur = temp;
        }
        cur_small->next = large->next;

        return small->next;
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
    def partition(self, head, x):
        """
        :type head: ListNode
        :type x: int
        :rtype: ListNode
        """
        # 两个链表分别保存对应的节点
        small = ListNode(-1)
        large = ListNode(-1)

        cur_s = small
        cur_l = large
        cur = head
        while cur:
            temp = cur.next
            cur.next = None
            if cur.val < x:
                cur_s.next = cur
                cur_s = cur
            else:
                cur_l.next = cur
                cur_l = cur
            cur = temp 

        cur_s.next = large.next
        return small.next
```

## 复杂度

代码只遍历了一次链表，时间复杂度是O(n)。由于只申请了有限个头节点的变量，空间复杂度是O(1)。

## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 