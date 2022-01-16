# leetcode 389. 找不同

## 题目先导

> 排序来排序去，数值虽有序，学习无高低。 —— by 胖胖

| 类型 | 难度 | 相关标签&基础知识 | 题目链接 |
| :------: | :--------: | :---: | :------: | 
| 算法 | 中等 | [字符串](#)、[排序](#) | [找不同](https://leetcode-cn.com/problems/find-the-difference/) | 

## 题目描述

```
给定两个字符串 s 和 t，它们只包含小写字母。

字符串 t 由字符串 s 随机重排，然后在随机位置添加一个字母。

请找出在 t 中被添加的字母。

示例 1：
输入：s = "abcd", t = "abcde"
输出："e"
解释：'e' 是那个被添加的字母。

示例 2：
输入：s = "", t = "y"
输出："y"

示例 3：
输入：s = "a", t = "aa"
输出："a"

示例 4：
输入：s = "ae", t = "aea"
输出："a"

提示：
    0 <= s.length <= 1000
    t.length == s.length + 1
    s 和 t 只包含小写字母

```

## 解题思路
挖掘题目重要信息：
1. 原始字符串s随机重排后，拆入一个字符c得到字符串t。根据这个信息，我们根据对s和t进行升序排列，依次比较s和t中的每个字符，找到第一个不相等的字符c就是最后的answer。

2. 统计s中每次字符出现的次数，由于t比s多出一个字符c，因此我们在遍历t的时候，依次减去每个字符出现的次数。由于只有26个小写字符，从26字符串中如果哪个字符出现次数小于0，则是插入的字符c。

3. 根据异或的性质进行计算：`a ^ a = 0, a ^ 0 = 0`。将s和t中所有的字符进行异或操作，最后的结果就是新拆入的字符c。

## 难点分析
该题目比较简单易懂，可以从更多的角度来解释。

## 代码
- 代码支持：C++、Python

**C++ Code**
```C++
class Solution {
public:
    char findTheDifference(string s, string t) {
        // 方案1. 先排序进行比较找到第一个不相等的字母就是添加的字符
        char ans = '0';
        sort(s.begin(), s.end());
        sort(t.begin(), t.end());
        int i = 0;
        for (i = 0; i < s.size(); i++) {
            if (s[i] != t[i]) {
                ans = t[i];
                break;
            }
        }
        if (i >= s.size()) {
            ans = t.back();
        }
        return ans;
    }
};

class Solution {
public:
    char findTheDifference(string s, string t) {
        // 方案2. 使用hash map，遍历字符串s记录每一个字符出现的次数；遍历t减去每次字符出现的次数，最后hash中出现次数是-1的就是添加的字符
        char ans = '0';
        int cnt[26] = {0};
        for (char c : s) {
            cnt[c - 'a'] ++;
        }
        for (char c : t) {
            cnt[c - 'a'] --;
        }
        for (int i = 0; i < 26; i++) {
            if (cnt[i] < 0) {
                ans = 'a' + i;
            }
        }
        return ans;
    }
};

class Solution {
public:
    char findTheDifference(string s, string t) {
        // 方案3. 异或性质：a ^ a = 0; a ^ 0 = a； ans = s ^ t
        int ans = 0;
        for (char c : s) {
            ans = ans ^ c;
        }
        for (char c : t) {
            ans = ans ^ c;
        }
        return (char) ans;
    }
};
```

**Python Code**
```Python

```

## 复杂度
方案1. 排序的时间复杂是`O(nlogn)`，遍历字符串时间复杂是`O(n)`。空间复杂是`O(1)`。

方案2. 遍历两次字符串，时间复杂度是`O(n)`，空间复杂度`O(26)`，常数空间，空间复杂度也是`O(1)`。

方案3. 遍历两次字符串，时间复杂度是`O(n)`，只使用了常数个中间遍历，空间复杂度是`O(1)`。

## PangLeetCode

Pang：来源自我家的猫老大叫胖胖，我们不定时给它们讲一些LeetCode算法题目，所以取名PangLeetCode。

大家对题目有任何的想法，随时邮件，或者加微信好友，随时交流。

邮箱: xiaoranone@gmail.com

微信: xiaoran-2 