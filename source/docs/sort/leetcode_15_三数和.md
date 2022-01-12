## Leetcode-15. 三数之和

## 题目先导

- 链接：https://leetcode-cn.com/problems/3sum
- 标签：数组、排序、双指针
- 基础知识：
  - 排序：
  - 双指针： 

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

题目中要求找到所有「不重复」且和为 00 的三元组，这个「不重复」的要求使得我们无法简单地使用三重循环枚举所有的三元组。这是因为在最坏的情况下，数组中的元素全部为 00，即排序+双指针。

## 难点提示
本题的难点在于如何去除重复解。



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
            if (i > 0 && nums[i] == nums[i-1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while (left < right) {
                if (left > i + 1 && nums[left] == nums[left-1]) {
                    left ++;
                    continue;
                }
                if (nums[i] + nums[left] + nums[right] < 0) {
                    left ++;
                }
                else if (nums[i] + nums[left] + nums[right] > 0){
                    right --;
                }
                else { // ==
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
        n = len(nums)
        nums.sort()
        ans = list()
        
        # 枚举 a
        for first in range(n):
            # 需要和上一次枚举的数不相同
            if first > 0 and nums[first] == nums[first - 1]:
                continue
            # c 对应的指针初始指向数组的最右端
            third = n - 1
            target = -nums[first]
            # 枚举 b
            for second in range(first + 1, n):
                # 需要和上一次枚举的数不相同
                if second > first + 1 and nums[second] == nums[second - 1]:
                    continue
                # 需要保证 b 的指针在 c 的指针的左侧
                while second < third and nums[second] + nums[third] > target:
                    third -= 1
                # 如果指针重合，随着 b 后续的增加
                # 就不会有满足 a+b+c=0 并且 b<c 的 c 了，可以退出循环
                if second == third:
                    break
                if nums[second] + nums[third] == target:
                    ans.append([nums[first], nums[second], nums[third]])
        
        return ans
```

## 复杂度分析
- 时间复杂度：$O(nlogn)$
- 空间复杂度：$O(1)$


## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给她们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 