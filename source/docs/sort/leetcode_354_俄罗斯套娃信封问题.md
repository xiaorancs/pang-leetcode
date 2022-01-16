# leetcode 354. 俄罗斯套娃信封问题

## 题目先导

> 排序来排序去，数值虽有序，学习无高低。 —— by 胖胖

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | [动态规划](#)、[排序](#) | [俄罗斯套娃信封问题](https://leetcode-cn.com/problems/russian-doll-envelopes/) | 

## 题目描述

```
给你一个二维整数数组 envelopes ，其中 envelopes[i] = [wi, hi] ，表示第 i 个信封的宽度和高度。

当另一个信封的宽度和高度都比这个信封大的时候，这个信封就可以放进另一个信封里，如同俄罗斯套娃一样。

请计算 最多能有多少个 信封能组成一组“俄罗斯套娃”信封（即可以把一个信封放到另一个信封里面）。

注意：不允许旋转信封。

 
示例 1：
输入：envelopes = [[5,4],[6,4],[6,7],[2,3]]
输出：3
解释：最多信封的个数为 3, 组合为: [2,3] => [5,4] => [6,7]。

示例 2：
输入：envelopes = [[1,1],[1,1],[1,1]]
输出：1
 
提示：
    1 <= envelopes.length <= 5000
    envelopes[i].length == 2
    1 <= wi, hi <= 10^4

```

## 解题思路
首先明确题目中所说，什么条件下才可以套娃。能够被进行嵌套的条件是两个信封a和b，满足`a.wi < b.wi && a.hi < b.hi`，因此我们需要先根据这个比较条件对所有信封进行排序。第二步，对已经排序的信封数组中，找到最长的可以嵌套的序列。到这里我们可以看到这个题目的原型是[最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)问题，我们值需要套用最长上升子序列，修改下比较方式即可。


最长上升子序列：
对于给定的数组arr，定义`dp[i]`：表示以第i位结尾的最长上升子序列的长度。`dp[i]`的最长状态可能来自任意`dp[1, j]`的状态，其中`j < i && arr[j] < arr[i]`，`dp[i] = max(dp[i], dp[j] + 1)`。

类似的，对于本题目，我们只需要修改下比较的条件：`j < i && arr[j].wi < arr[i].wi && arr[j].hi < arr[i].hi`


## 难点分析
本题目的难点，在于我们是否能够本题目和历史学习过的题目最长上升子序列联系起来，其实很多题目都是类似，知识改变了一些说法。

对于最长上升子序列问题，还有一个O(nlogn)的解法，就是维护一个有序的数组，每次通过二分插入的数组中，有兴趣的同学可以研究下。


## 代码
- 代码支持：C++、Python

**C++ Code**
```C++
class Solution {
public:
    int maxEnvelopes(vector<vector<int>>& envelopes) {
        int ans = 1;
        // 先升序排列
        sort(envelopes.begin(), envelopes.end());
        vector<int> dp(envelopes.size(), 0);
        // 找到可以套娃的最多信封
        for (int i = 0; i < envelopes.size(); i++) {
            dp[i] = 1;
            // cout << i << " " << envelopes[i][0] << " " << envelopes[i][1] << endl;
            for (int j = 0; j < i; j++) {
                if (envelopes[j][0] < envelopes[i][0] && envelopes[j][1] < envelopes[i][1]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
                ans = max(ans, dp[i]);
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
在求解题目的我们首先使用了快速排序对数组进行原地排序，使用的时间是`O(nlogn)`，求解最长信封数的时候，使用了两层循环，时间复杂度是`O(n^2)`，最后的时间复杂度是`O(n^2)`。

由于开辟了一个数组存在中间的状态，因此空间复杂度是`O(n)`。

## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 