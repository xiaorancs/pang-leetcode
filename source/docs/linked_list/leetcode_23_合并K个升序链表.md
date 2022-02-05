# 23. 合并K个升序链表


| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 困难 | [链表](#)、[排序](#)、[堆](#) | [合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/) | 

## 题目描述

```
给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

示例 1：
    输入：lists = [[1,4,5],[1,3,4],[2,6]]
    输出：[1,1,2,3,4,4,5,6]
    解释：链表数组如下：
    [
        1->4->5,
        1->3->4,
        2->6
    ]
    将它们合并到一个有序链表中得到。
    1->1->2->3->4->4->5->6

示例 2：
    输入：lists = []
    输出：[]

示例 3：
    输入：lists = [[]]
    输出：[]

提示：
    k == lists.length
    0 <= k <= 10^4
    0 <= lists[i].length <= 500
    -10^4 <= lists[i][j] <= 10^4
    lists[i] 按 升序 排列
    lists[i].length 的总和不超过 10^4

```

## 解题思路

题目要求合并k个有序的链表，我们需要面临的主要问题是：k个链表的当前k个节点选择出一个最小的节点进行合并。我们的问题就变成了每次都k个节点中选择出一个最小的节点，小根堆可能帮助我们解决这个问题，小根堆是每次返回最小的那个元素，因此我们只需要维护一个大小是k个小根堆即可，每次从小根堆中选择一个节点进行合并，并将这个节点的下一个节点插入到小根堆中，直到遍历所有的链表节点。

## 难点分析
难点1、对于多个元素的比较中选择出最大或者最小的一个，需要能想到堆或者优先队列这个数据结构进行解题。


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
    struct cmp {
        // 小根堆的比较方式
        bool operator()(ListNode* a, ListNode* b) {
            return a->val > b->val;
        }
    };
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.empty()) return NULL;
        // 保存k个链表节点的小根度，
    	priority_queue<int, vector<ListNode*>, cmp> heap;
        for (ListNode* node : lists) {
            if (node) heap.push(node);
        }
        ListNode* head = new ListNode(0);
        ListNode* cur = head;
        while (!heap.empty()) {
            ListNode* tmp = heap.top();
            heap.pop();
            if (tmp) {
                cur->next = tmp;
                cur = cur->next;            
            }
            if (tmp && tmp->next) {
                tmp = tmp->next;
                heap.push(tmp);
            }
        }
        return head->next;
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

import heapq

class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        if not lists:
            return None
        hp = []
        # 维护k大小的小根堆
        for l in lists:
            if l:
                heapq.heappush(hp, (l.val, l))
        # 定义哑节点
        dummy = ListNode(-1)
        cur = dummy
        while hp:
            _, t = heapq.heappop(hp)
            if t:
                cur.next = t
                cur = cur.next
            if t and t.next:
                t = t.next
                heapq.heappush(hp, (t.val, t))
        return dummy.next
```

## 复杂度

堆的时间复杂度是O(logk)，每次从k个元素中选择出最小的元素的时间复杂度是O(logk)，每次插入一个节点的时间复杂度也是O(logk)。我们假设一共花有n个节点，因此总的时间复杂度是O(2n*logk)。

空间复杂度是O(k)，最多需要维护k个大小的堆。

## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 