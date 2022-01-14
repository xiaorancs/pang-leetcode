
## 题目先导

> 排序来排序去，数值虽有序，学习无高低。 —— by 胖胖

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | [双指针](##)、[排序](##) | [三数和](https://leetcode-cn.com/problems/3sum) | 

## 题目描述

```
给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

示例 1：
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]

示例 2：
输入：nums = []
输出：[]

示例 3：
输入：nums = [0]
输出：[]
 

提示：

    0 <= nums.length <= 3000
    -10^5 <= nums[i] <= 10^5
```

## 解题思路

题目要求找到三个数a，b，c，且满足`a + b + c = 0`。比较直观的思路就是：使用三层循环找到所有三元组的组合，判断每个三元组是否满足条件，找到所有三元组和等于0的目标即可，但是不幸的是，这样的算法会超时，因为三层循环的时间负责度O(n^3)，题目中给的数组最大长度是3000，O(n^3)肯定过不了，O(n^2)时间复杂度大概率是能够通过的。

我们对题目进行变化，如果a固定的情况下，目前就是找到两个数的和等于-a即可。之前我们都做过类似的题目：[两数之和](https://leetcode-cn.com/problems/two-sum/)。如果题目不要求找到所有的三元组组合，我们就可以是使用hash表来计算两数之和，时间复杂度是O(n)，再算上外层的循环O(n)，时间复杂度是O(n^2)。

如果数组是**有序**的计算两个之和不使用hash表，也能在O(n)的时间复杂度完成，使用双指针从两端向中间探测。


因此三数和问题被我们拆分成两个部分：

1. 固定一个a，问题变成再数组中找到`b + c = -a`的两数之和问题。
2. 两数之和问题，我们先对数组排序，再使用双指针两头向中间探测，解决两数之和问题。
3. 总结：先排序，固定a，使用双指针解两数之和等于-a的问题。

使用快速排序将整数数据有序，之后固定a，使用使用双指针解两数之和等于-a的二元组。使用两个指针left和right，有三种种情况：

1. `nums[left] + nums[right] < -a` --> `left = left + 1`
2. `nums[left] + nums[right] > -a` --> `right = right - 1`
2. `nums[left] + nums[right] = -a`；保存 `<left, right>`，`left = left + 1, right = right - 1`

## 难点分析
本题的难点在于如何在得到所有解的同时，且不包含重复解。如果去重重复解，目标是相等的数值使用一次，因此已经排序。对于第一层循环，如果`num[i] = nums[i-1]`，则跳过，因为`nums[i-1]`的时候已经使用过了，不在重试使用。

第二个可能重复的地方是，当计算两数之和的时候，如果`nums[left] = nums[left-1]`同理也要跳过。

留一个问题：为什么right不用跳过相等？


## 代码
- 代码支持：C++、Python

**C++ Code**
```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> ans;
        if (nums.size() < 3) return ans;
        sort(nums.begin(), nums.end());
        // 最关键的是如何去重
        for (int i = 0; i <= nums.size()-3; i++) {
            // 重复的数字值使用一次
            if (i > 0 && nums[i] == nums[i-1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while (left < right) {
                // 重复数字只使用一次，因为nums[left]+nums[right]=-a,因此left跳过相当于right跳过
                if (left > i + 1 && nums[left] == nums[left-1]) {
                    left ++;
                    continue;
                }
                if (nums[i] + nums[left] + nums[right] < 0) {
                    left ++;
                } else if (nums[i] + nums[left] + nums[right] > 0){
                    right --;
                } else { 
                    ans.push_back({nums[i], nums[left], nums[right]});
                    left ++;
                    right --;
                }
            }
        } 
        return ans;
    }
};
```

**Python Code**
```Python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        if len(nums) < 3:
            return []
        nums.sort()
        # print(nums)
        ans = []
        for i in range(len(nums)):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            a = nums[i]
            left = i + 1
            right = len(nums) - 1
            while left < right:
                if left > i + 1 and nums[left] == nums[left - 1]:
                    left += 1
                    continue
                if nums[left] + nums[right] < -a:
                    left += 1
                elif nums[left] + nums[right] > -a:
                    right -= 1
                else:
                    ans.append([a, nums[left], nums[right]])
                    left += 1
                    right -= 1
        return ans
```

## 复杂度分析

排序的时间复杂度是`O(nlog)`，找到三元组的时间复杂度是`O(n^2)`，因此时间复杂度是`O(n^2)`。由于只用到常数的空间，因此空间复杂度是`O(1)`。

## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给她们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 