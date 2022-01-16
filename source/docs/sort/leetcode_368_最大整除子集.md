# leetcode 368. 最大整除子集
## 题目先导

> 排序来排序去，数值虽有序，学习无高低。 —— by 胖胖

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | [动态规划](#)、[排序](#) | [最大整除子集](https://leetcode-cn.com/problems/largest-divisible-subset/) | 

## 题目描述

```
给你一个由 无重复 正整数组成的集合 nums ，请你找出并返回其中最大的整除子集 answer ，子集中每一元素对 (answer[i], answer[j]) 都应当满足：
answer[i] % answer[j] == 0 ，或
answer[j] % answer[i] == 0
如果存在多个有效解子集，返回其中任何一个均可。

示例 1：
输入：nums = [1,2,3]
输出：[1,2]
解释：[1,3] 也会被视为正确答案。

示例 2：
输入：nums = [1,2,4,8]
输出：[1,2,4,8]
 
提示：
    1 <= nums.length <= 1000
    1 <= nums[i] <= 2 * 10^9
    nums 中的所有整数 互不相同

```

## 解题思路
本题目要求找出最大整除子集，整除子集的定义是：每一元素对 `(answer[i], answer[j])`都应当满足：`answer[i] % answer[j] == 0`，或`answer[j] % answer[i] == 0`。

我们定义`dp[i]`表示以`nums[i]`结尾的某个整除子集，`dp[i]`的状态来自任意`j in [1, i-1]`的区间，使用dp[j]来更新dp[i]。

当满足条件`nums[i] % nums[j] == 0` 或 `nums[j] % nums[i] == 0` 的时候，我们更新`dp[i] = max(dp[j] + 1, dp[i])`。

到了这里，可以很多小伙伴已经看出来了，这个题目也是[最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)的变形，只要修改下比较方式就可以解决了。

上述的判断条件因为不知道`nums[i]`和`nums[j]`的大小关系，因此需要判断两次，我们如果对nums进行升序排列，只需要判断一个`nums[i] % nums[j]`是否等0即可，而且此时我们能够保证如果`nums[i] % nums[j] == 0`等时候，一定存在`nums[i]`和整个子集都满足整除关系。


## 难点分析
难点1. 必须首先进行升序排序，这样才能够保证，如果新增的nums[i]可以整除其前一个数值，则保证整个子集都是可以整除的。

难点2. 是否能够看出本题目也是最长上升子序列题目的变形。

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

题目首先进行快速排序，时间复杂度是O(nlogn)；其次，使用两层循环进行计算，时间复杂度是O(n^2)，最后的时间复杂度是O(n^2)。

由于开辟了空间存在中间状态，空间复杂度是O(n+n)=O(2n)，最后的空间复杂度是O(n)。


## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 