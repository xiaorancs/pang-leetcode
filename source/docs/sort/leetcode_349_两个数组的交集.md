# leetcode 349. 两个数组的交集
## 题目先导

> 排序来排序去，数值虽有序，学习无高低。 —— by 胖胖

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | 数组、[归并排序](#) | [两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/) | 

## 题目描述

```
给定两个数组，编写一个函数来计算它们的交集。

示例 1：
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]

示例 2：
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]

说明：
    输出结果中的每个元素一定是唯一的。
    我们可以不考虑输出结果的顺序。

```

## 解题思路
方案1、排序之后，同时遍历两个数组，依次比较每一个值，找到重复的元素，整个过程类似于归并排序的一个归并的过程。时间复杂度是`O(nlogn)`，空间复杂度是`O(1)`。

方案2、使用hash map。首页遍历数组arr_a，将所有出现过的值保存到hash表中，之后再遍历数组arr_b，依次判断每一个值是否在hash map中出现。时间复杂度是`O(n)`，空间复杂度是`O(n)`。

## 难点分析
这个问题我们需要思考，什么时候使用方案1，什么时候使用方案2。一般情况下，我们需要根据当前机器的运行瓶颈进行判断，如果当前机器cpu是瓶颈，内存很充足，则使用时间上更快的方案2；但是如果当前机器内存不足，为了不增加内存，我们可能需要考虑使用更节省空间的方案1。


## 代码
- 代码支持：C++、Python

**C++ Code**
```C++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        // 方案1
        vector<int> ans;
        sort(nums1.begin(), nums1.end());
        sort(nums2.begin(), nums2.end());
        int i = 0;
        int j = 0;
        while (i < nums1.size() && j < nums2.size()) {
            if (nums1[i] < nums2[j]) {
                i++;
            } else if (nums1[i] > nums2[j]) {
                j++;
            } else {
                // 去重
                if (ans.size() == 0 || ans.back() != nums1[i]) {
                    ans.push_back(nums1[i]);
                }
                i++;
                j++;
            }
        }
        return ans;
    }
};

class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        // 方案2
        unordered_set<int> hash;
        vector<int> ans;
        for (auto x : nums1) {
            hash.insert(x);
        }
        for (auto x : nums2) {
            if (hash.find(x) != hash.end()) {
                ans.push_back(x);
                // 去重
                hash.erase(x);
            }
        }
        return ans;
    }
};
```

**Python Code**
```Python

```

## 复杂度
方案1的时间瓶颈主要在排序的时间瓶颈，排序需要消耗O(nlogn)的时间复杂度，一次归并的计算耗时是O(n)，因此时间复杂度是O(nlog)，空间复杂度是O(1)。

方案2的时间复杂度主要是遍历数组，时间复杂度是O(n)，由于我们需要开辟一个空间存储hash表，因此空间复杂度是O(n)。

## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 