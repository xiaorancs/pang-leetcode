# leetcode 148. 排序链表

## 题目先导

> 排序来排序去，数值虽有序，学习无高低。 —— by 胖胖

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | [链表](#)、[排序](#) | [排序链表](https://leetcode-cn.com/problems/sort-list/) | 

## 题目描述

```
给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

示例 1：
输入：head = [4,2,1,3]
输出：[1,2,3,4]
解释：
    origin list: 4->2->1->3
    sorted list: 1->2->3->4

示例 2：
输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]
解释：
    origin list: -1->5->3->4->0
    sorted list: -1->0->3->4->5

示例 3：
输入：head = []
输出：[]
 

提示：
    链表中节点的数目在范围 [0, 5 * 10^4] 内
    -10^5 <= Node.val <= 10^5
 

进阶：你可以在 O(n log n) 时间复杂度和常数级空间复杂度下，对链表进行排序吗？

```

## 解题思路
我们使用三种方案对链表进行排序：

方案1、插入排序
- 始终维护前i个有序的链表
- 对于第i+1个元素，在前i个有序的链表中找到如何的位置，拆入第i+1个元素

方案2、归并排序
- 将元素的链表等分成两部分a和b，对于链表就是找到原始链表的中间节点mid节点。为了找到中间节点，有两个方案。
  - 遍历数组得到数据的长度n，找到n/2个节点，中间节点
  - 使用快慢指针找到中间节点，快指针每次走两步，慢指针每次走一步，快指针到达尾结点，慢指针刚好到达中间节点
- 递归调用对a和b进行排序
- 对排行好的a和b进行merge
  
方案3、快速排序



## 难点分析


## 代码
- 代码支持：C++、Python

**C++ Code**
```C++
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        // 排序
        sort(nums.begin(), nums.end());
        int ans = 0;
        int index = -1;
        vector<int> dp(nums.size(), 0);
        vector<int> father(nums.size(), -1);
        for (int i = 0; i < nums.size(); i++) {
            dp[i] = 1;
            for (int j = 0; j < i; j++) {
                if (nums[i] % nums[j] == 0 && dp[i] < dp[j] + 1) {
                    dp[i] = dp[j] + 1;
                    // 记录i来自哪个状态
                    father[i] = j;
                }
            }
            if (ans < dp[i]) {
                index = i;
                ans = dp[i];
            }
        }
        // cout << index << " " << ans;
        vector<int> result;
        while (index != -1) {
            result.push_back(nums[index]);
            index = father[index];
        }
        return result;
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