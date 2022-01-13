## 题目先导

> 排序来排序去，数值虽有序，学习无高低。 —— by 胖胖

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | 数组、[滑动窗口](##) | [插入区间](https://leetcode-cn.com/problems/insert-interval/) | 

## 题目描述

```
给你一个 无重叠的 ，按照区间起始端点排序的区间列表。
在列表中插入一个新的区间，你需要确保列表中的区间仍然有序且不重叠（如果有必要的话，可以合并区间）。

示例 1：
输入：intervals = [[1,3],[6,9]], newInterval = [2,5]
输出：[[1,5],[6,9]]

示例 2：
输入：intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
输出：[[1,2],[3,10],[12,16]]
解释：这是因为新的区间 [4,8] 与 [3,5],[6,7],[8,10] 重叠。

示例 3：
输入：intervals = [], newInterval = [5,7]
输出：[[5,7]]

示例 4：
输入：intervals = [[1,5]], newInterval = [2,3]
输出：[[1,5]]

示例 5：
输入：intervals = [[1,5]], newInterval = [2,7]
输出：[[1,7]]
 

提示：
    0 <= intervals.length <= 10^4
    intervals[i].length == 2
    0 <= intervals[i][0] <= intervals[i][1] <= 10^5
    intervals 根据 intervals[i][0] 按 升序 排列
    newInterval.length == 2
    0 <= newInterval[0] <= newInterval[1] <= 10^5
```

## 解题思路


## 难点分析


## 代码
- 代码支持：C++、Python

**C++ Code**
```C++

```

**Python Code**
```Python

```

## 复杂度
- 时间复杂度：
- 空间复杂度：


## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 