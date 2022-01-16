## 题目先导

> 排序来排序去，数值虽有序，学习无高低。 —— by 胖胖

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | 数组、[排序](#) | [插入区间](#) | 

## 题目描述

```

```

## 解题思路


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