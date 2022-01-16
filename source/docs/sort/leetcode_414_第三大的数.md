# leetcode 414. 第三大的数

## 题目先导

> 排序来排序去，数值虽有序，学习无高低。 —— by 胖胖

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | 数组、[排序](#) | [第三大的数](https://leetcode-cn.com/problems/third-maximum-number/) | 

## 题目描述

```
给你一个非空数组，返回此数组中 第三大的数 。如果不存在，则返回数组中最大的数。

示例 1：
输入：[3, 2, 1]
输出：1
解释：第三大的数是 1 。

示例 2：
输入：[1, 2]
输出：2
解释：第三大的数不存在, 所以返回最大的数 2 。

示例 3：
输入：[2, 2, 3, 1]
输出：1
解释：注意，要求返回第三大的数，是指在所有不同数字中排第三大的数。
此例中存在两个值为 2 的数，它们都排第二。在所有不同数字中排第三大的数为 1 。

提示：
    1 <= nums.length <= 10^4
    -2^31 <= nums[i] <= 2^31 - 1

```

## 解题思路

方案1. 先对数组进行降序排序，直接找到第三大的数即可。

方案2. 如果是求数组中的最大的数，我们就不会进行排序，而是使用一个数字，依次比较元素找到元素的最大值。因为题目要求找第三大的数，我们可以使用三个数字first、second、third依次找到前三大的数字，最后返回third。


## 难点分析
方案1：题目要求是第3大的数，不能重复，比如`3,3,2,1`；其第三大的数是1，不是降序排序的第3个位置。因此对于方案1，我们不能在排序之后取第3个位置的作为结果，需要进行遍历找到不重复的第3大的数。

方案2：我们需要保存三个变量`first,second,third`来保存前三个最大值数，但是如果这个数组中只有两个数或者一个数的时候，我们需要判断third是否被赋值，如果没有被赋值就直接返回first。为了解决这个我们，我们将`first,second,third`初始化成一个不可能出现的值即可，如果最后发现third是原始的初始值则直接返回first。但是题目的提示中显示`-2^31 <= nums[i] <= 2^31 - 1`，我们需要选择一个不在这个范围中的数字，一个方法就是选择这个范围之外的值；第二个方法是赋值成一个浮点数，最后返回结果的时候再转回整数。


## 代码
- 代码支持：C++、Python

**C++ Code**
```C++
class Solution {
public:
    int thirdMax(vector<int>& nums) {
        // 方案1. 排序
        sort(nums.begin(), nums.end(), [](const int a, const int b){
            return a >  b;
        });
        int k = 1;
        int ans = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            if (nums[i] != nums[i-1]) {
                k++;
            }
            if (k == 3) {
                ans = nums[i];
                break;
            }
        }
        return ans;        
    }
};

class Solution {
public:
    int thirdMax(vector<int>& nums) {
        // 方案2. 因为题目要求第三大的数，我们只需要保存前三大的数即可
        if (nums.size() == 1) {
            return nums[0];
        } 
        if (nums.size() == 2) {
            return max(nums[0], nums[1]);
        }
        // 使用double 1.1 表示未被赋值
        double first = 1.1;
        double second = 1.1;
        double third = 1.1;
        for (int i = 0; i < nums.size(); i++) {
            if (first == 1.1 || nums[i] > first) {
                third = second;
                second = first;
                first = nums[i];
            } else if (nums[i] < first && (second == 1.1 || nums[i] > second)) {
                third = second;
                second = nums[i];
            } else if (nums[i] < second && (third == 1.1 || nums[i] > third)) {
                third = nums[i];
            }
        }
        // cout << first << endl;
        // cout << second << endl;
        // cout << third << endl;        
        int ans = third == 1.1 ? (int) first : (int) third;
        return ans;
    }
};
```

**Python Code**
```Python

```

## 复杂度
方案一、需要先进行排序，再进行遍历，时间复杂度是`O(nlogn)+O(n)=O(nlogn)`，空间复杂度是`O(1)`。

方案二、只有一次遍历的比较，时间复杂是`O(n)`，只使用了三个中间变量，空间复杂度是`O(1)`。

## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 