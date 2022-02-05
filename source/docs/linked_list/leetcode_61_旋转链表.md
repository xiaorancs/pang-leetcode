# 61. 旋转链表


| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | [链表](#) | [旋转链表](https://leetcode-cn.com/problems/rotate-list/) | 

## 题目描述

```
给你一个链表的头节点 head ，旋转链表，将链表每个节点向右移动 k 个位置。
```

```
示例 1：
    输入：head = [1,2,3,4,5], k = 2
    输出：[4,5,1,2,3]

```
![示例 1](https://assets.leetcode.com/uploads/2020/11/13/rotate1.jpg)
```
示例 2：
    输入：head = [0,1,2], k = 4
    输出：[2,0,1]
```
![示例 2](https://assets.leetcode.com/uploads/2020/11/13/roate2.jpg)


```
提示：
    链表中节点的数目在范围 [0, 500] 内
    -100 <= Node.val <= 100
    0 <= k <= 2 * 109
```

## 解题思路

题目思路：假设链表的长度是n，将每个节点向右移动k个位置，保证k < n。
例如：
```
head = [1,2,3,4,5], k = 2, n = 5

第一次移动：[5,1,2,3,4]

第二次移动：[4,5,1,2,3]

从数据的角度将每个节点向右一定k个位置，相当于将最后k个节点移动到链表的头部。
```

因此解决这个问题需要两步：
- 找到倒数第k个节点的开头，将这k个节点移动到链表的头部
- 由于k可能大于链表长度n，依次我们需要知道链表的长度n，k=k%n。
- 当我们知道两部长度之后，为了找到倒数第k个节点，想当于找到链表的正数第n-k个节点


## 难点分析

难点1、注意k的长度可能大于链表的长度，需要先求得链表的长度n。

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

    int get_length(ListNode* head, ListNode*& pre) {
        int n = 0;
        ListNode* cur = head;
        pre = cur;
        while (cur) {
            n += 1;
            pre = cur;
            cur = cur->next;
        }
        return n;
    }

    ListNode* rotateRight(ListNode* head, int k) {
        if (!head) return head;
        ListNode* last;
        int n = get_length(head, last);
        k = k % n;
        k = n - k;
        if ( k == n) {
            return head;
        }
        ListNode* cur = head;
        ListNode* pre = cur;
        while (k) {
            pre = cur;
            cur = cur->next;
            k --;
        }
        pre->next = NULL;
        last->next = head;
        return cur;
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
    def length(self, head):
        len = 0
        pre = None
        cur = head
        while cur:
            len += 1
            pre = cur
            cur = cur.next
        return len, pre

    def rotateRight(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        if not head or k == 0:
            return head
        n, last = self.length(head)
        k = k % n
        t = n - k
        # 如果是最后一个则不需要移动
        if t == n:
            return head
        pre, cur = None, head
        # 找到第n-k个节点
        while t > 0:
            t -= 1
            pre = cur
            cur = cur.next

        pre.next = None
        last.next = head

        return cur
```

## 复杂度

题目需要首先计算链表的长度，此时的时间复杂度是O(n)。之后需要找到第n-k个节点，需要的时间是O(n-k)，因此总的时间复杂度是O(n)。

空间复杂度是O(1)。

## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 