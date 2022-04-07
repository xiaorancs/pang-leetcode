<!-- TOC -->

- [链表](#链表)
  - [插入](#插入)
    - [介绍](#介绍)
    - [力扣真题](#力扣真题)
  - [查找](#查找)
    - [力扣真题](#力扣真题-1)
  - [修改](#修改)
    - [力扣真题](#力扣真题-2)
  - [删除](#删除)
    - [介绍](#介绍-1)
    - [力扣真题](#力扣真题-3)
  - [反转](#反转)
    - [介绍](#介绍-2)
    - [力扣真题](#力扣真题-4)
  - [旋转](#旋转)
    - [介绍](#介绍-3)
    - [力扣真题](#力扣真题-5)
  - [合并](#合并)
    - [介绍](#介绍-4)
    - [力扣真题](#力扣真题-6)
  - [相交](#相交)
    - [介绍](#介绍-5)
    - [力扣真题](#力扣真题-7)
  - [环形链表](#环形链表)
  - [链表排序](#链表排序)
  - [循环链表](#循环链表)
  - [双向链表](#双向链表)
  - [跳跃链表](#跳跃链表)

<!-- /TOC -->
# 链表
链表是一种线性数据结构，它通过指针将多个元素组合在一起，形成一个链表，一般都需要存在头节点和尾结点。为了表示链表的结束，一般将最后节点指向空节点，为了更好的处理头节点，在链表的头节点之前保留一个哑结点指向头节点。一般我们说到链表都是指单链表，主要有两个部分组成，一个是next节点，指向下一个节点，一个是当前节点存储的数据。

```c++
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}
};
```

## 插入

### 介绍
1. 头插法
每次插入到链表的头部。

初始化链表：dummy_node -> null_node。
> dummy_node: 称为伪头结点或者哑铃节点

- 插入新节点new_a_node：dummy_node -> new_a_node -> null_node。
- 插入新节点new_a_node：dummy_node -> new_b_node -> new_a_node -> null_node。

为了插入新的节点，new_x_node = new ListNode(1); 我们需要维持2个当前的头节点（head_node）、dummy_node。 
- dummy_node->next = head_node；要保持哑铃节点指向头节点，初始化的时候进行**一次**操作。
- new_x_node->next = head_node；新节点头插，指向当前头节点head_node。
- head_node = new_x_node；插入任何新节点之后，始终保持head_node是指向头结点。

```C++
ListNode* headInsert() {
    // 定义哑铃节点，此时dummy_node->next = NULL;
    ListNode* dummy_node = new ListNode(-1);
    // 定义头节点，当前头节点是NULL
    ListNode* head_node = nullptr;
    dummy_node->next = head_node;

    // 使用头插法插入依次[1,2,3,4]4个节点
    int i = 1;
    while (i <= 4) {
        // 新建节点new_node
        ListNode* new_node = new ListNode(i);
        new_node->next = head_node;
        head_node = new_node;
    }
    return dummy_node->next;
}

```

1. 尾插法

初始化链表：dummy_node -> null_node

每次将新节点插入到链表的尾部。

- 插入新节点new_a_node：dummy_node -> new_a_node -> null_node。
- 插入新节点new_b_node：dummy_node -> new_a_node -> new_b_node -> null_node。

为了插入新的节点，new_x_node = new ListNode(1); 我们需要维持2个当前的最后一个节点（tail_node）。
- new_x_node->next = tail_node->next；新插入的节点要先指向tail_node的后继节点，目的是为了防止链表断链。
- tail_node->next = new_x_node；当new_x_node节点指向tail_node的全部后续节点之后，将tail_node节点后继指向新插入的节点new_x_node。
- tail_node = new_x_node；维护tail_node始终指向最后一个节点

```C++
ListNode* headInsert() {
    // 定义哑铃节点，此时dummy_node->next = NULL;
    ListNode* dummy_node = new ListNode(-1);
    // 维护当前尾结点
    ListNode* tail_node = dummy_node;
    // 使用头插法插入依次[1,2,3,4]4个节点
    int i = 1;
    while (i <= 4) {
        // 新建节点new_node
        ListNode* new_node = new ListNode(i);
        new_node->next = tail_node;
        tail_node->next = new_node;
        tail_node = new_node;
    }
    return dummy_node->next;
}
```
注意，由于我们使用了dummy_node节点，因此单链表的头节点应该是：dummy_node->next。

### 力扣真题


## 查找

这里提到的链表都表示单链表，后续在介绍双链表、循环链表等。这里的查找也在指在单链表上的查找。链表的数据结构是通过指针将所有元素组织成一个链表，因此查找第k个节点、倒数第k个节点或者指定的元素值，我们只能从头节点开始，依次向后查找并进行比较才行。如果想要知道一个链表的长度，也需要进行一个遍历，时间复杂度是O(n)。

我们有如下单链表：
```
1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7 -> null
```

目标1、查找链表的第k个节点

```C++
ListNode* findFirstK(ListNode* head, int k) {
    // 依次遍历链表，找到第k个节点
    ListNode* current = head;
    while (current != NULL && k > 0) {
        current = current->next;
        --k;
    }
    // 这块有两种情况，
    // 1. k == 0, 退出循环，表示找到的第k个节点
    // 2. current == null, 退出循环，表示链表结束，还没找到第k个节点，即len(head) < k，此时放回的是null节点。
    return current;
}
```
结论1、这种从头进行遍历查找，还可以支持以下类型的查找：
- 查找链表中是否存在指定的值value，只要再遍历的时候一次比较。
- 得到链表的长度，直到链表到达空节点，记录长度。


目标2、查找链表倒数第k个节点
> 思路1. 先遍历一遍链表，得到链表的长度length；再查找链表的第length-k个节点。
> 思路2. 使用双指针fast和slow，fast指针先向前走k步之后，slow指针从头开始，之后每一步fast和slow指针都向后走一步，当fast指针到达链表尾部的时候，slow的指针恰好指向倒数第k个节点。

```C++
ListNode* findLastK(ListNode* head, int k) {
    // 依次遍历链表，找到第k个节点
    ListNode* current = head;
    ListNode* fast = head;
    ListNode* slow = head;
    while (fast != NULL) {
        while (k--) {
            fast = fast->next;
        }
        fast = fast->next;
        slow = slow->next;
    }
    return slow;
}
```
结论2、这个双指针的查找思路，还可以进行变化支持下面的查找问题。
- 找到链表的中间节点，也可以使用双指针fast和slow，fast指针每次都两步，slow指针每次走一步，当fast指针达到尾部，slow刚好执行链表的中间节点。

### 力扣真题

1. [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)
```
输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。

示例：
给定一个链表: 1->2->3->4->5, 和 k = 2.
返回链表 4->5.
```

题解：我们前面提到过解倒数第k个节点的方法，双指针法，fast和slow，fast先走k步，slow再跟着一起走，当fast到达链表尾部，则slow是倒数第k个节点。数学证明：
```
length: 链表长度
当fast走了length步，由于fast比slow先走k步，则slow走了length-k步，恰好在倒数第k个节点。
```

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
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (fast) {
            while (k-- > 0) fast = fast->next;
            if (!fast) break;
            fast = fast->next;
            slow = slow->next;
        }
        return slow;
    }
};
```

2. [876. 链表的中间结点](https://leetcode-cn.com/problems/middle-of-the-linked-list/)

```
给定一个头结点为 head 的非空单链表，返回链表的中间结点。
如果有两个中间结点，则返回第二个中间结点。

示例 1：

输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.

示例 2：

输入：[1,2,3,4,5,6]
输出：此列表中的结点 4 (序列化形式：[4,5,6])
由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。
```

题解：我们提到查找问题，可以先遍历链表得到链表的长度；上一个题目我们使用了双指针的解决，一次遍历得到倒数第k个节点。这个题目我们依然从双指针的角度进行思考，中间节点是链表的一半的位置，使用双指针fast和slow，如果当fast到达链表尾部的时候，slow恰好指向中间节点，相当于fast比slow走的快了一倍，因此我们只需要每次让fast走两步，slow走一步，则当fast走到尾部，slow指向中间节点。

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
    ListNode* middleNode(ListNode* head) {
        // 快慢指针
        ListNode* slow;
        ListNode* fast;
        slow = fast = head;
        while (fast && fast->next) {
            fast = fast->next->next;
            slow = slow->next;
        }
        return slow;
    }
};
```

## 修改

单链表的从常规修改和查找基本思路一致，一般情况下所有的修改都要先查找到，才能进行修改对应值。

常规的修改问题如下：
1. 将所有value=1的节点的值，修改为value=2。
   - 根据查找的架构，找到所有value=1的节点，修改即可。
2. 交换指点两个节点的value值，交换第k个节点和倒数第k个节点的value值。
   - 根据前面的查找架构，先找到第k个节点first_k_node和倒数第k个节点last_k_node。
   - 交换value：t = first_k_node->val, first_k_node->val = last_k_node->val, last_k_node->val = t。

非常规的修改问题，一般不是修改对应的value值，而且修改链表的指向，常见的问题如下：

1. 奇偶拆分，将链表的所有value是奇数的节点移动到链表的前半部分，value是偶数节点，移动到后半部分。
   - 移动之后，保留两个分区中每个节点的初始相对位置。
   - 移动之后，不保留两个分区中每个节点的初始相对位置。
2. 分隔链表，给的一个值x，使得所有小于x的节点都出现在大于或等于 x 的节点之前。
   - 移动之后，保留两个分区中每个节点的初始相对位置。
   - 移动之后，不保留两个分区中每个节点的初始相对位置。
3. 将指定区间[a, b]的链表，移动到链表的尾部或者头部。
4. 交换链表的节点，比如两两交换其中相邻的节点。

题目1和题目2是类似的问题，只要再比较的时候稍作转化即可，我们这里以题目1为例，先分析下问题，
- 不保证每个节点的初始位置的情况下，值是偶数的节点，放到链表的后半部分；值是奇数的节点放回链表的前半部分。
  - 关注值是奇数的节点，遍历链表，如果当前节点current_node的值是奇数节点，我们将其current_node插入到链表的头部即可。
  - 如果我们关注偶数节点，遍历链表，如果当前节点current_node的值是偶数节点，我们将其current_node插入到链表的尾部可以吗？这个要做很多预处理，因为你当已经满足所有偶数节点都到后部分的时候，你需要帮助合适退出训练。读者可以仔细思考下。
  - **总结**，对于不这种区分成两部分的问题，我们只需要将满足前部分要求的节点，一次插入到链表头部即可，其实本质是在考察链表的插入。

- 如果要保证每个节点的初始位置呢，这种情况下，我们一个比较简单的思路就是依次遍历，维护两个头指针first_node和second_node，first_node将所有奇数节点串联起来，second_node将所有偶数节点串联起来，最后再合并即可。
  - 遍历整个链表，
    - 如果当前节点current_node->val 是奇数，first_node->next = current_node, first_node = first_node->next。
    - 如果当前节点current_node->val 是奇数，second_node->next = current_node, second_node = second_node->next。
    - 合并first_node和second_node, 将first_node的尾部指向second_node链表的头部即可。

题目3. 将指定的链表的区间范围[a, b]移动到链表的头部。
- 将链表拆分成3个部分[1, pre_a] + [a, b] + [suf_b, end]
- 因此我们遍历的时候，需要找到b, pre_a, 
- pre_a->next = b->next; b->next = head; dummy->next = a

### 力扣真题
1. [86. 分隔链表](https://leetcode-cn.com/problems/partition-list/)

```
给你一个链表的头节点 head 和一个特定值 x ，请你对链表进行分隔，使得所有 小于 x 的节点都出现在 大于或等于 x 的节点之前。

你应当 保留 两个分区中每个节点的初始相对位置。
```

示例 1：
![示例1](https://assets.leetcode.com/uploads/2021/01/04/partition.jpg)
```
输入：head = [1,4,3,2,5,2], x = 3
输出：[1,2,2,4,3,5]
```
示例 2：
```
输入：head = [2,1], x = 2
输出：[1,2]
```

题解：题目要求保留两个分区中每个节点的初始相对位置。为了保留这个位置，我们只能从头到尾依次遍历链表，在遍历的过程中，组织好前后的两部分，最后将两部分相链接，整体分析之后，可以发现这个题目其实考查的是链表的尾插法。
> 如果cur->val < x, 则插入到first指针后面。
> 
> 如果cur->val >= x, 则插入到seconde指针后面。

注意，要时刻记得设置dummy_node，这里有两个链表，我们设置联合节点。

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
        ListNode* dummy_first = new ListNode(-1);
        ListNode* dummy_second = new ListNode(-1);
        ListNode* first = dummy_first;
        ListNode* second = dummy_second;
        ListNode* current = head;
        while (current) {
            ListNode* next = current->next;
            if (current->val < x) {
                first->next = current;
                first = first->next;
            } else {
                second->next = current;
                second = second->next;
            }
            current->next = nullptr;
            current = next;
        }
        // 链接两个部分
        first->next = dummy_second->next;
        return dummy_first->next;
    }
};
```

2. [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

```
给定单链表的头节点head，将所有索引为奇数的节点和索引为偶数的节点分别组合在一起，然后返回重新排序的列表。

第一个节点的索引被认为是 奇数 ， 第二个节点的索引为偶数 ，以此类推。

请注意，偶数组和奇数组内部的相对顺序应该与输入时保持一致。

你必须在O(1)的额外空间复杂度和O(n)的时间复杂度下解决这个问题。
```
示例 1:
![示例1](https://assets.leetcode.com/uploads/2021/03/10/oddeven-linked-list.jpg)
```
输入: head = [1,2,3,4,5]
输出: [1,3,5,2,4]
```
示例 2:
![示例2](https://assets.leetcode.com/uploads/2021/03/10/oddeven2-linked-list.jpg)
```
输入: head = [2,1,3,5,6,4,7]
输出: [2,3,6,7,1,5,4]
```

题解，这个题目的思路和前一个题目一样，解题架构也是类似，只需要修改下判断条件即可。将 `current->val > x`修改为`index % 2 == 1`即可。

```
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
    ListNode* oddEvenList(ListNode* head) {
        ListNode* dummy_first = new ListNode(-1);
        ListNode* dummy_second = new ListNode(-1);
        ListNode* first = dummy_first;
        ListNode* second = dummy_second;
        ListNode* current = head;
        int index = 1;
        while (current) {
            ListNode* next = current->next;
            if (index % 2 == 1) {
                first->next = current;
                first = first->next;
            } else {
                second->next = current;
                second = second->next;
            }
            ++index;
            current->next = nullptr;
            current = next;
        }
        // 链接两个部分
        first->next = dummy_second->next;
        return dummy_first->next;
    }
};
```

## 删除

### 介绍
删除一般要求删除指定的节点，或者删除满足条件的部分节点。我们要时刻想着链表这个数据结构的性质，如果我们想要删除节点a，必须要知道a的前一个节点pre_a。如果删除的节点是头节点一定要特殊处理，一般我们要使用dummy节点，dummy->next = head。

常见的删除类型如下：
1. 给定一个值value，删除所有value的节点。
   - 定义哑铃节点，指向链表头节点
   - 遍历链表，记录pre和current节点，pre是current节点的前驱
   - 删除current节点之前，一定要记录下下一个节点next
   - 删除指定节点，pre->next = current->next
2. 给定一个待删除的节点del_node，删除这个节点。
   - 找到del_node节点的前驱节点pre_node
   - 删除del_node, pre->next = del_node。
   - 或者将del_node的值和del_node->next进行交换，删除del_node->next节点：del_node->next = del_node->next->next。
3. 删除指定位置的节点，例如删除第k个节点，或者删除中间节点。
    - 例如删除第k个节点，需要先找到第k-1个节点。
    - 如果要删除中间节点，可以先使用前面提到的快慢指针方法找到中间节点，但是要多保存下来中间节点的前驱节点，进行删除。
4. 删除有序链表中的重复的节点，例如重复的节点只保留一个，或者删除所有重复的节点。
    - 删除排序链表中的重复节点，只保留一个 
      - [1,1,2,4] 删除之后 [1,2,4]
      - 判断是否是重复节点，如果cur->val == cur->next->val，删除cur->next节点。cur->next = cur->next->next。
    - 删除排序链表中所有的重复节点，一个不留
      - [1,1,2,4] 删除之后 [2,4]
      - 对于每一个当前节点cur，记录前驱pre节点，我们需要判断cur->val是否等于cur->next->val。
      - 如果相等，则向后探测找到所有相等的节点，删除这段链表。
      - 如果不相等，则正常更新。

一定要时刻牢记两个点，找到待删除节点的前一个节点；一定要优先设置哑铃节点。

```C++
ListNode* deleteNode(ListNode* head, int val) {
    ListNode* dummy = new ListNode(-1);
    dummy->next = head;
    ListNode* pre = dummy;
    ListNode* current = head;

    while (current) {
        // 删除current节点，删除之前要记录下一个节点，
        ListNode* next = current->next;
        if (current->val == val) {
            pre->next = current->next;
        }
        // 每次都要更新前驱节点
        pre = current;
        current = next;
    }
    return dummy->next;
}
```

### 力扣真题
1. [19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)
```
给你一个链表，删除链表的倒数第n个结点，并且返回链表的头结点。
```
示例 1：
![示例1](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)
```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
示例 2：

输入：head = [1], n = 1
输出：[]
示例 3：

输入：head = [1,2], n = 1
输出：[1]
```

题解：双指针法删除倒数第n个节点，注意两个点：
1. 设置哑铃节点dummy_node
2. 双指针fast和slow，fast先走n步，slow再跟着一起走
3. 删除指定节点，一定要知道该节点的前驱节点

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(-1, head);
        ListNode* prev = dummy;
        ListNode* fast = head;
        ListNode* slow = head;
        while (fast) {
            while (n-- > 0) fast = fast->next;
            if (!fast) break;
            fast = fast->next;
            prev = slow;
            slow = slow->next;
        }
        prev->next = slow->next;
        return dummy->next;
    }
};
```
2. [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

```
给定一个已排序的链表的头head，删除所有重复的元素，使每个元素只出现一次。返回 已排序的链表。
```

示例 1：
![](https://assets.leetcode.com/uploads/2021/01/04/list1.jpg)
```
输入：head = [1,1,2]
输出：[1,2]
```
示例 2：
![](https://assets.leetcode.com/uploads/2021/01/04/list2.jpg)
```
输入：head = [1,1,2,3,3]
输出：[1,2,3]
```

题解：删除节点cur，必须要知道cur的前驱节点。因为是排序的链表，因此为了判断重复，我们比较cur->val 和 cur->next->val即可，如果存在相同的值，则删除cur->next，因此此时我们已经知道它的前驱节点是cur。
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
3. [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

```
给定一个已排序的链表的头 head ， 删除原始链表中所有重复数字的节点，只留下不同的数字 。返回 已排序的链表 。
```
示例 1：
![](https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg)
```
输入：head = [1,2,3,3,4,4,5]
输出：[1,2,5]
```
示例 2：
![](https://assets.leetcode.com/uploads/2021/01/04/linkedlist2.jpg)
```
输入：head = [1,1,1,2,3]
输出：[2,3]
```

题解：删除所有重复的节点，我们需要找到重复的区间范围。例如val存在重复，当判断到`cur->val == cur->next->val`时候，cur->val一定会存在重复，我们要首先得到重复的区间范围，即向后探测看有多少个cur->val。找到区间范围[a, b]，并删除[a, b]。

需要注意的点：时刻使用哑铃节点，如果删除的范围存在头节点则可以直接处理。

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
        ListNode* dummy_head = new ListNode(-1, head);
        ListNode* pre = dummy_head; 
        while (cur != nullptr && cur->next != nullptr) {
            // 不重复重复，正常更新
            if (cur->val != cur->next->val) {
                pre = cur;
                cur = cur->next;
            } else { // 存在重复，找到重复的区间范围
                int val = cur->val;
                while (cur && cur->val == val) {
                    cur = cur->next;
                }
                // 删除重复节点
                pre->next = cur;
            }
        }
        return dummy_head->next;
    }
};
```

4. [237. 删除链表中的节点](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)

```
请编写一个函数，用于 删除单链表中某个特定节点 。在设计函数时需要注意，你无法访问链表的头节点head，只能直接访问 要被删除的节点 。
题目数据保证需要删除的节点 不是末尾节点 。
```

示例 1：
![](https://assets.leetcode.com/uploads/2020/09/01/node1.jpg)
```
输入：head = [4,5,1,9], node = 5
输出：[4,1,9]
解释：指定链表中值为5的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9
```

示例 2：
![](https://assets.leetcode.com/uploads/2020/09/01/node2.jpg)
```
输入：head = [4,5,1,9], node = 1
输出：[4,5,9]
解释：指定链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9
```
题解：删除指定节点node，一定要知道其前一个节点，一个正常的方案是找到node节点的前驱节点，进行删除。另一个方式，是将node节点的值和node->next的节点进行交换，删除node->next节点即可。

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
    void deleteNode(ListNode* node) {
        // 交换值
        swap(node->val, node->next->val);
        // 删除node->next
        node->next= node->next->next;
    }
};
```

## 反转

### 介绍
链表的反转，一般是指将整个链表或者链表中的一部分进行翻转。例如将整个链表翻转：
```
整个翻转：
原始链表：1->2->3->4->null
翻转链表：4->3->2->1->null

部分反转：例如反转索引在[2,3]范围的节点。
原始链表：1->2->3->4->null
翻转链表：1->3->2->4->null
```
链表的反转有两种方案：
1. 头插法翻转
   - 优先设置哑铃节点dummy，dummy->next=head。
   - 遍历链表，从第二个节点进行头插法，相关代码如下。
    ```C++
    ListNode* reverseList(ListNode* head) {
        // 先不管有没有，设置个哑铃节点
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        
        ListNode* current = head;
        // 从第二个节点进行头插法
        while (current && current->next) {
            // 将current->next节点插入到头部
            ListNode* next = current->next;
            // 链接当前节点current和next->next的节点，为了下次遍历
            current->next = next->next;
            // next插入到头部节点，头部节点现在是dummy->next
            next->next = dummy->next;
            dummy->next = next;
        }
        return dummy->next;
    }
    ``` 
2. 三指针遍历翻转
   - 三指针反转，是指在遍历的过程中，将所有指针的next指针全部进行反转
   - 因为反转的时候，我们需要维护current，当前节点的前一个节点prev，和后一个节点next，因此称为三指针。
    ```C++
    ListNode* reverseList(ListNode* head) {
        // 三指针法，因为返回的头节点是之前的尾结点，暂时不用设置哑铃节点
        ListNode* prev = NULL;
        LIstNode* current = head;
        while (current) {
            ListNode* next = current->next;
            // 反转: prev, current, next
            current->next = prev;
            prev = current;
            current = next;
        }
        return prev;
    }
    ```
### 力扣真题

1. [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)
```
给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。
 
```
示例 1：
![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)
```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```
示例 2：
![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)
```
输入：head = [1,2]
输出：[2,1]
```

题解：我们可以使用两种解法，一个头插法，一个是三指针法。这里我们使用头插法进行反转，从第2个节点一次开始插入到头部。注意设置哑铃节点。
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
    ListNode* reverseList(ListNode* head) {
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;

        ListNode* cur = head;
        ListNode* pre = dummy;
        while (cur && cur->next != nullptr) {
            ListNode* next = cur->next;
            // 先记录下一个节点
            cur->next = next->next;
            // 使用头插法将next节点插入到头部
            next->next = pre->next;
            pre->next = next;

        }
        return dummy->next;
    }
};
```

2. [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)
```
给你单链表的头指针 head 和两个整数 left 和 right ，其中 left <= right 。请你反转从位置 left 到位置 right 的链表节点，返回 反转后的链表 。
```
示例 1：
![](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)
```
输入：head = [1,2,3,4,5], left = 2, right = 4
输出：[1,4,3,2,5]
```

题解：我们还是使用头插法，因为头插法相比三指针法逻辑比较清晰。题目要求反转[left, right]范围的链表，因为我们对left-1进行正常变量，接下对right-left个节点使用头插法进行反转即可。

题解2：我们先找到第left个节点和第right的节点，写一个函数对left_node到right_node的链表进行反转，并返会新的头尾节点。这里我们建议使用头插法，因为头插法的最后一个节点就是反转后的新的头节点，原来的头节点就是反转后新的尾节点。

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
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        // 对于指定区间进行头插法进行逆转
        ListNode* dummy = new ListNode(-1, head);
        ListNode* pre = dummy;
        // 1. 知道left之前的第一个元素
        int i = 0;
        while (i < left-1) {
            pre = pre->next;
            i++;
        }
        ListNode* cur = pre->next;
        i = 0;
        while (i < right-left) {
            // 对cur节点的next节点插入到left的位置，
            // 插入之前记录下一个节点，进行遍历。
            ListNode* cur_next = cur->next;
            ListNode* temp = cur_next->next;
            // 头插反转
            cur_next->next = pre->next;
            pre->next = cur_next;
            // 连接下一个继续进行
            cur->next = temp;
            i++;
        }
        return dummy->next;
    }
};

// 方案2:
class Solution {
public:
    pair<ListNode*, ListNode*> reverseUtil(ListNode* left, ListNode* right) {
        ListNode* pre = nullptr;
        ListNode* cur = left;
        while (pre != right) {
            ListNode* next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }
        return {right, left};
    }
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        // 对于指定区间进行头插法进行逆转
        ListNode* dummy = new ListNode(-1, head);
        ListNode* pre = dummy;
        ListNode* left_node_prefix = dummy;
        ListNode* left_node = NULL;
        ListNode* right_node = NULL;    
        ListNode* right_node_suffix = NULL;
        ListNode* cur = head;   
        // 1. 知道left之前的第一个元素
        int i = 1;
        while (cur) {
            if (i == left-1) {
                left_node_prefix = cur;
            }
            if (i == left) {
                left_node = cur;
            }
            if (i == right) {
                right_node = cur;
                break;
            }
            ++i;
            cur = cur->next;
        }
        right_node_suffix = right_node->next;
        // 反转left -> right，并返回新的头尾节点
        tie(left_node, right_node) = reverseUtil(left_node, right_node);
        // 链接
        left_node_prefix->next = left_node;
        right_node->next = right_node_suffix;
        return dummy->next;
    }
};
```


3. [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)
```
给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。
k 是一个正整数，它的值小于或等于链表的长度。
如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

进阶：
你可以设计一个只使用常数额外空间的算法来解决此问题吗？
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。
```

示例 1：
![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)
```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

示例 2：
![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)
```
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```

题解：意思很清楚，每次找到k个长度的链表[head, tail]进行反转，并返回新的头尾节点，和原来的链表进行链接。有以下几个点注意：
- 写一个函数，反转[head, tail]并返回新的头尾节点，这个函数在上个题目中的第二种解法，我们已经实现。
- 维护一个指针prev是[head, tail]的前驱节点，进行链接；链接之后prev挑战到新的tail节点，继续遍历。

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
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        ListNode* pre = dummy;
        while (head) {
            // 这里一定要使用新的变量
            ListNode* tail = pre;
            // 找到k长度的链表
            for (int i = 0; i < k; i++) {
                tail = tail->next;
                if (!tail) { // 不满足k
                    return hair->next;
                }
            }
            // 反转之前记录下一个节点，下次遍历使用
            ListNode* next = tail->next;
            // 反转
            tie(head, tail) = reverse(head, tail);
            // 链接新的头尾节点
            pre->next = head;
            tail->next = next;
            // 维护pre节点，跳转到当前k范围的tail
            pre = tail;
            // 继续迭代
            head = next;
        }
        return dummy->next;
    }
};
```


4. [2074. 反转偶数长度组的节点](https://leetcode-cn.com/problems/reverse-nodes-in-even-length-groups/)

```
给你一个链表的头节点 head 。

链表中的节点 按顺序 划分成若干 非空 组，这些非空组的长度构成一个自然数序列（1, 2, 3, 4, ...）。一个组的 长度 就是组中分配到的节点数目。换句话说：

节点 1 分配给第一组
节点 2 和 3 分配给第二组
节点 4、5 和 6 分配给第三组，以此类推
注意，最后一组的长度可能小于或者等于 1 + 倒数第二组的长度 。

反转 每个 偶数 长度组中的节点，并返回修改后链表的头节点 head 。
```

示例 1：
![](https://assets.leetcode.com/uploads/2021/10/25/eg1.png)
```
输入：head = [5,2,6,3,9,1,7,3,8,4]
输出：[5,6,2,3,9,1,4,8,3,7]
解释：
- 第一组长度为 1 ，奇数，没有发生反转。
- 第二组长度为 2 ，偶数，节点反转。
- 第三组长度为 3 ，奇数，没有发生反转。
- 最后一组长度为 4 ，偶数，节点反转。
```

示例 2：
![](https://assets.leetcode.com/uploads/2021/10/25/eg2.png)
```
输入：head = [1,1,0,6]
输出：[1,0,1,6]
解释：
- 第一组长度为 1 ，没有发生反转。
- 第二组长度为 2 ，节点反转。
- 最后一组长度为 1 ，没有发生反转。
```

题解：这个题目和上个题目每k个反转一组类似，只是将k改成一个动态变化的数据，其他基本一致，唯一不同的是最后一段的处理，如果最后一段链表的个数是奇数则不反转。以下需要注意的点：
- 一个函数，反转[head, tail]并返回新的头尾节点
- 遍历过程维护上一个节点进行链接，遍历过程中记录k个链表的个数。
- 最后一组要进行特殊处理

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
    // 反转头插法
    pair<ListNode*, ListNode*> reverse(ListNode* head, ListNode* tail) {
        ListNode* prev = nullptr;
        ListNode* cur = head;
        while (prev != tail) {
            ListNode* next = cur->next;
            cur->next = prev;
            prev = cur;
            cur = next;
        }
        return {tail, head};
    }

    ListNode* reverseEvenLengthGroups(ListNode* head) {
        int k = 1;
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        ListNode* pre = dummy;
        ListNode* cur = nullptr;
        while (head) {
            // 每次找k个，k是动态增加的，每轮增加1。
            ListNode* tail = pre;
            for (int i = 0; i < k; i++) {
                cur = tail;
                tail = tail->next;
                // 特殊处理最后一组，最后一组可能不足k个，因此要记录最后一组的个数
                if (!tail) { 
                    tail = cur;
                    k = i;
                    break;
                }
            }
            // 偶数个反转
            if (k % 2 == 0) {
                ListNode* next = tail->next;
                tie(head, tail) = reverse(head, tail);
                pre->next = head;
                tail->next = next;
                pre = tail;
                head = next;
            } else { // 奇数不反转，其他一致
                ListNode* next = tail->next;
                pre = tail;
                head = next;
            }
            ++k;
        }
        return dummy->next;
    }
};
```

## 旋转

### 介绍
旋转一般是对线性结构的操作，我们知道数组上的旋转，这里说的是链表上的旋转，将链表的每个节点向右旋转k个位置。  

例如：将下面单链表向右旋转2个位置。
```
1->2->3->4->5->null
第1次旋转：5->1->2->3->4->null
第2次旋转：4->5->1->2->3->null
```
从题目中可以看到，向右旋转k次，可以转化为倒数后k个节点，移动到链表的头部。
- 使用查找中给出的方法，找打低k个节点last_k，和最后一个节点last。因为最后一个节点要指向链表的头部head。
- last->k_1->next = null; last->next = head; 新的头节点是last_k。

### 力扣真题
1. [61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)

```
给你一个链表的头节点 head ，旋转链表，将链表每个节点向右移动 k 个位置。
```


示例 1：
![](https://assets.leetcode.com/uploads/2020/11/13/rotate1.jpg)
```
输入：head = [1,2,3,4,5], k = 2
输出：[4,5,1,2,3]
```
示例 2：
![](https://assets.leetcode.com/uploads/2020/11/13/roate2.jpg)
```
输入：head = [0,1,2], k = 4
输出：[2,0,1]
```

题解：所有节点向右移动k个位置，可以转成成最后k个节点移动到链表的头部。`[first_k, last_k] --> [last_k, first_k]`。需要注意一点k大于链表的长度length，这里要进行取余`k=k%length`。因此这个题目拆分成下面两步：
- 链表长度，得到需要移动的最后k个节点。
- 将最后k个节点移动到链表头部。
  - 找到倒数第k个节点last_k（正数第length-last_k）和最后一个节点last
  - last->next = head, return last_k


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
    // 得到链表的长度和链表的最后一个节点
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
        // 找到last_k节点
        ListNode* cur = head;
        ListNode* pre = cur;
        while (k) {
            pre = cur;
            cur = cur->next;
            k --;
        }
        // 移动到头部
        pre->next = NULL;
        last->next = head;
        return cur;
    }
};
```

## 合并
### 介绍
合并一般指两个或者两个以上的链表的操作，所有这里我们将所有关于两个的链表都称为合并操作。

常见的题目：
1. 合并两个有序的链表
   - 我们将合并进行转换操作，每次我们从比较两个链表a和b，将更小的节点插入新的链表的尾部。
   - 申明dummy节点，依次链接较小的节点接口。
   - 必须声明的一点，两个链表不会都是到达链表尾部，需要单独处理剩下的链表。
2. 合并k个有序的链表
   - 多个有序的链表、和两个没有本质区别，唯一的区别是，我们如何从k个节点中找到最小的那个节点插入到新的链表的尾部。
   - 这里我们会用到最小堆的数据结构，维护一个k大小的最小堆，每次弹出一个最小节点进行插入。
3. 两数相加，两个链表表示的整数进行相加，返回一个新的链表的和。
   - 这个题目其实就是模型数学上的加法，我们依然是使用一个dummy节点，每次插入一个新节点
   - 需要注意的是，模拟加法的时候，要注意进位
   - 注意处理剩下的链表，并且要注意处理最后一位是不是有进位操作。


### 力扣真题
1. [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
```
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
```

示例 1：
![](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)
```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
示例 2：
```

题解：每次比较l1和l2的大小，将小的节点插入到新的链表头节点。这个问题就转化成了链表的插入，我们应该使用尾插法，先声明一个哑铃节点。

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
        ListNode* dummy = new ListNode(0);
        ListNode* head = dummy;
        while (l1 && l2) {
            if (l1->val < l2->val) {
                head->next = l1;
                l1 = l1->next;
            } else {
                head->next = l2;
                l2 = l2->next; 
            }
            head = head->next;
        }
        // 对于两个以上链表或者数组的操作，一定要处理全部
        if (l1) head->next = l1;
        if (l2) head->next = l2;
        return dummy->next;
    }
};
```

2. [23. 合并K个升序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

```
给你一个链表数组，每个链表都已经按升序排列。
请你将所有链表合并到一个升序链表中，返回合并后的链表。
```
 
示例 1：
```
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
```

题解：这个题目和两个链表合并基本一致，我们要学会联想相关或者相似的题目，唯一不同的点是如果在每次比较中从k个链表当前节点中选择出最小的节点，从k个元素中选择出最小的元素，这是最小堆解决的问题。因此这个题目就是在比较的时候引入最小堆，其他执行链表的尾插法。

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
        // 定义小根堆的比较方式
        bool operator()(ListNode* a, ListNode* b) {
            return a->val > b->val;
        }
    };
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.empty()) return NULL;
        // 保存k个链表节点的小根度，
    	priority_queue<int, vector<ListNode*>, cmp> heap;
        // 维护k个节点的小顶堆，先将所有链表的头节点放入小顶堆
        for (ListNode* node : lists) {
            if (node) heap.push(node);
        }
        // 哑铃节点
        ListNode* dummy = new ListNode(0);
        ListNode* cur = dummy;
        // 每次从堆中弹出最小的节点，进行插入，直到全部处理
        while (!heap.empty()) {
            ListNode* tmp = heap.top();
            heap.pop();
            // 如果当前节点不是空节点则进行插入
            if (tmp) {
                cur->next = tmp;
                cur = cur->next;            
            }
            // 如果当前节点存在下一个节点，则下一个节点进堆
            if (tmp && tmp->next) {
                tmp = tmp->next;
                heap.push(tmp);
            }
        }
        return dummy->next;
    }
};
```

3. [445. 两数相加 II](https://leetcode-cn.com/problems/add-two-numbers-ii/)

```
给你两个 非空 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。
```

示例1：

![](https://pic.leetcode-cn.com/1626420025-fZfzMX-image.png)
```
输入：l1 = [7,2,4,3], l2 = [5,6,4]
输出：[7,8,0,7]
```

题解：题目用链表表示数字，我们可以模型加法操作，加法操作是从低位相加，多10进1。题目中数字的最高位在链表头部，为了从低位开始模拟加法，我们要首先对链表进行反转。之后模拟加法，并申请一个新的头节点进行插入，注意一定不要忘记处理最后的进位。最后再对相加后的链表进行反转。
- 写一个反转函数
- 模拟加法，并进行尾插法

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

    ListNode* reverse(ListNode* head) {
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        ListNode* current = head;
        // 从第二个节点进行头插法
        while (current && current->next) {
            // 将current->next节点插入到头部
            ListNode* next = current->next;
            // 链接当前节点current和next->next的节点，为了下次遍历
            current->next = next->next;
            // next插入到头部节点，头部节点现在是dummy->next
            next->next = dummy->next;
            dummy->next = next;
        }
        return dummy->next;
    }

    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        l1 = reverse(l1);
        l2 = reverse(l2);

        ListNode* dummy = new ListNode(-1);
        ListNode* cur = dummy;
        int flag = 0;
        while (l1 && l2) {
            int s = l1->val + l2->val + flag;
            flag = s / 10;
            ListNode* new_head = new ListNode(s % 10);
            cur->next = new_head;
            cur = cur->next;
            l1 = l1->next;
            l2 = l2->next;
        }
        // 尾处理
        while (l1) {
            int s = l1->val + flag;
            flag = s / 10;
            ListNode* new_head = new ListNode(s % 10);
            cur->next = new_head;
            cur = cur->next;
            l1 = l1->next;
        }
        // 尾处理
        while (l2) {
            int s = l2->val + flag;
            flag = s / 10;
            ListNode* new_head = new ListNode(s % 10);
            cur->next = new_head;
            cur = cur->next;
            l2 = l2->next;
        }
        // 最后进位处理
        if (flag) {
            ListNode* new_head = new ListNode(1);
            cur->next = new_head;
            cur = cur->next;
        }
        // 反转
        auto head = reverse(dummy->next);
        return head;
    }
};
```

## 相交
### 介绍
相交一般是形容两个链表的，两个单链表是否存在相交的情况，如果相交，返回第一个公共节点。
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

上图中链表A和链表B相交于c1节点，当给出两个表头HeadA和HeadB，我们可以使用以下的方法找打第一个相交的节点c1。

方案1、使用hash表，先遍历整个链表A，记录在hash表中，再遍历链表B，B链表中第一个在hash表中出现的节点就是第一个相交的节点。时间复杂度O(len_A+len_B)，空间复杂度O(len_A)。

方案2、根据两个链表的长度推算出相交的节点。链表A和链表B的长度分别是len_A和len_B，长的链表先走abs(len_A - len_B)步，之后两个链表一起走，遇到的第一个相同的节点就是相交的节点。为了计算时间复杂度，我们将链表分成如下，介绍相交的部分是c，链表A不相交的部分是a，链表B不想交的部分是b。首先要知道链表的长度需要时间O(len_A+len_B)，再找到第一个相交节点O(max(a, b))，总的时间负责度是O(len_A+len_B+max(a,b))。

方案3、是在2的基础上进行改进，方案2中，我们将链表分成3部分，可以进行如下转化：a+c+b = b+c+a。等式的左边a+c表示当前链表A到达尾部，将其移动到链表B头继续遍历，直到遇到相交节点。

### 力扣真题
1. [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

```
给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 null 。

图示两个链表在节点 c1 开始相交：
```
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)
```
题目数据 保证 整个链式结构中不存在环。

注意，函数返回结果后，链表必须 保持其原始结构 。

自定义评测：

评测系统 的输入如下（你设计的程序 不适用 此输入）：

intersectVal - 相交的起始节点的值。如果不存在相交节点，这一值为 0
listA - 第一个链表
listB - 第二个链表
skipA - 在 listA 中（从头节点开始）跳到交叉节点的节点数
skipB - 在 listB 中（从头节点开始）跳到交叉节点的节点数
评测系统将根据这些输入创建链式数据结构，并将两个头节点 headA 和 headB 传递给你的程序。如果程序能够正确返回相交节点，那么你的解决方案将被 视作正确答案 。
```
 

示例 1：
![](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,6,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

示例 2：

![](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)
```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 。
```

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
    // 使用上面介绍的方案3
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* currA = headA;
        ListNode* currB = headB;
        while (currA != currB) {
            if (currA) currA = currA->next; 
            else currA = headB;
            if (currB) currB = currB->next;
            else currB = headA;
        }
        return currB;
    }
};
```

## 环形链表

单链表一般是线性结构，我们可以理解为直线。但是有的链表可能存在环形结构，即链表的最后一个节点不是指向空节点，而是指向到链表的某一个节点，则形成的环形结构。我们需要判断一个单链表是否存在环形结构，如果存在还要确定第一个进行环形的节点。

## 链表排序


## 循环链表


## 双向链表


## 跳跃链表

