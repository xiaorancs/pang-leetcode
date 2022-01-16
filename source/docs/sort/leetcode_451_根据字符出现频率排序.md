# leetcode 451. 根据字符出现频率排序

## 题目先导

> 排序来排序去，数值虽有序，学习无高低。 —— by 胖胖

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | [哈希](#)、[排序](#) | [根据字符出现频率排序](https://leetcode-cn.com/problems/sort-characters-by-frequency/) | 

## 题目描述

```
给定一个字符串，请将字符串里的字符按照出现的频率降序排列。

示例 1:
输入: "tree"
输出: "eert"
解释:
'e'出现两次，'r'和't'都只出现一次。
因此'e'必须出现在'r'和't'之前。此外，"eetr"也是一个有效的答案。

示例 2:
输入: "cccaaa"
输出: "cccaaa"
解释:
'c'和'a'都出现三次。此外，"aaaccc"也是有效的答案。
注意"cacaca"是不正确的，因为相同的字母必须放在一起。

示例 3:
输入: "Aabb"
输出: "bbAa"

解释:
此外，"bbaA"也是一个有效的答案，但"Aabb"是不正确的。
注意'A'和'a'被认为是两种不同的字符。

```

## 解题思路
首先使用hash，统计每个字符出现的次数。根据所有单词组成pair<count, char>；再根据pair.first的进行排序。之后根据次数组合成字符串。

## 难点分析
时间复杂度和空间复制分析。

## 代码
- 代码支持：C++、Python

**C++ Code**
```C++
class Solution {
public:
    string frequencySort(string s) {
        unordered_map<char, int> counts;
        for (char c : s) {
            if (counts.find(c) == counts.end()) {
                counts[c] = 0;
            }
            counts[c] ++;
        }
        vector<pair<int, char>> vec;
        for (auto data : counts) {
            vec.push_back({data.second, data.first});
        }
        sort(vec.begin(), vec.end(), [](const pair<int, char> a, const pair<int, char> b){
            return a.first > b.first;
        });
        string ans = "";
        for (auto data : vec) {
            string t(data.first, data.second);
            // cout << t << endl;
            ans += t;
        }
        return ans;
    }
};
```

**Python Code**
```Python

```

## 复杂度
时间复杂度是遍历一个字符串需要时间复杂度是`O(n)`，排序的时间复杂度是`O(nlogn)`，遍历hash进行组合字符串时间复杂度是`O(128)`，这里假设只有128个字符，总的时间复杂度是`O(nlogn)`。

空间复杂度是`O(128)`，假设最多有128个字符，空间复杂度是固定的，可以认为是空间复杂度是`O(1)`。

## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 