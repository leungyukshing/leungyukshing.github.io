---
title: LeetCode解题报告（450）-- 1180. Count Substrings with Only One Distinct Letter
mathjax: true
date: 2021-12-24 22:46:23
---

## Problem

Given a string `s`, return *the number of substrings that have only **one distinct** letter*.

<!-- more -->

**Example 1:**

```
Input: s = "aaaba"
Output: 8
Explanation: The substrings with one distinct letter are "aaa", "aa", "a", "b".
"aaa" occurs 1 time.
"aa" occurs 2 times.
"a" occurs 4 times.
"b" occurs 1 time.
So the answer is 1 + 2 + 4 + 1 = 8.
```

**Example 2:**

```
Input: s = "aaaaaaaaaa"
Output: 55
```

**Constraints:**

- `1 <= s.length <= 1000`
- `s[i]` consists of only lowercase English letters.

------

## Analysis

&emsp;&emsp;这道题目是easy，暴力可以求解，但是这里我想讨论dp的做法。对于substring的dp，这题是一道很好的入门题，substring的dp一般会把状态方程`dp[i]`定义为以`i`结尾的子串。这里题目的要求是字串中只能有一个字符，那么我们只需要和前一个`i - 1`比就可以了。如果相同说明字符的种类没有增加，所以`dp[i] = dp[i - 1] + 1`，加的这个1是以`i`这个字符结尾的串，如果不同说明要重新来，所以就是1了（也可以把默认值都设置成1）。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int countLetters(string s) {
        int size = s.size();
        vector<int> dp(size, 1);
        int result = 1;
        for (int i = 1; i < size; ++i) {
            if (s[i] == s[i - 1]) {
                dp[i] = dp[i - 1] + 1;
            }
            result += dp[i];
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是substring dp的入门题，我觉得是值得记录的。这道题目的分享到这里，感谢你的支持！
