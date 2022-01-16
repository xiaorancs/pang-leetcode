# leetcode 347. 前 K 个高频元素

## 题目先导

> 排序来排序去，数值虽有序，学习无高低。 —— by 胖胖

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | 数组、[排序](#)、[堆排序](#)| [前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)| 

## 题目描述

```
给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。

示例 1:
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]

示例 2:
输入: nums = [1], k = 1
输出: [1]
 
提示：
    1 <= nums.length <= 105
    k 的取值范围是 [1, 数组中不相同的元素的个数]
    题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的
 

进阶：你所设计算法的时间复杂度 必须 优于 O(n log n) ，其中 n 是数组大小。

```

## 解题思路

题目要求返回出现频率前 k 高的元素：我们可以拆分成两步，第一步统计每个元素出现的次数；第二步根据每次元素的次数进行排序，返回top k的元素。

第一步：统计每个元素的次数，使用map的数据结构，遍历一次数组arr，记录每个元素的个数。

第二步：根据频次进行排序

- 方案1、将map中的所有元素组合成pair<key, counts>存入到一个新数组arr_count，对arr_count使用快速排序。
- 方案2、维护一个根据频次排序且size是k个大根堆，依次遍历整个map进入到大根堆中，返回最后大小是k的大根堆。


## 难点分析
该题目的解题思路比较清洗，主要难点是如果不让你使用一些库函数，自己实验大根堆或者快速排序的算法，我理解是本题的难点。因此也希望大家在使用基础的数据结构的时候，能够熟练掌握其原理。

## 代码
- 代码支持：C++、Python

**C++ Code**
```C++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> count_map;
        // 统计每次元素的频次
        for (auto x : nums) {
            if (count_map.find(x) == count_map.end()) {
                count_map[x] = 0;
            }
            count_map[x] ++;
        }
        // 方案1：快速排序
        vector<pair<int, int>> vec_count;
        for (auto data : count_map) {
            vec_count.push_back({data.first, data.second});
        }
        sort(vec_count.begin(), vec_count.end(), [](pair<int, int> a, pair<int, int> b){
            return a.second > b.second;
        });
        // top k结果
        vector<int> ans;
        for (int i = 0; i < min(k, (int)vec_count.size()); i++) {
            ans.push_back(vec_count[i].first);
        }
        return ans;
    }
};


class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> count_map;
        // 统计每次元素的频次
        for (auto x : nums) {
            if (count_map.find(x) == count_map.end()) {
                count_map[x] = 0;
            }
            count_map[x] ++;
        }
        // 方案2：priority_queue -> 小根堆
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> heap_max;
        for (auto data : count_map) {
            // cout << "data: " << data.first << " " << data.second << endl;
            heap_max.push({data.second, data.first});
            if (heap_max.size() > k) {
                // cout << "heap: " << heap_max.top().first << " " << heap_max.top().second << endl;
                heap_max.pop();
            }
        }
        // top k
        vector<int> ans;
        while (heap_max.size() > 0) {
            ans.push_back(heap_max.top().second);
            heap_max.pop();
        }
        return ans;
    }
};

```

**Python Code**
```Python

```

## 复杂度
本体的两种解法的时间复杂都是`O(nlogn)`，第一步遍历数组时间复杂度是`O(n)`，第二步如果使用快速排序，我们知道快速排序的时间复杂度是`O(nlogn)`，堆的时间复杂也是`O(nlog)`。

但是空间复杂度是不一样的，对于方案你，我们需要维护一个map和一个新的数组用来进行排序，使用两个O(n)的空间，因此空间复杂度是`O(n+n)`。如果使用大根堆进行存储，我们需要维护大小是k的堆即可，空间复杂度是`O(n+k)`。


## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 