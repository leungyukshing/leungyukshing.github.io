---
title: LeetCode解题报告（272）-- 5. Longest Palindromic Substring
tags:
  - LeetCode
mathjax: true
abbrlink: 45711
date: 2021-01-31 21:49:18
---

## Problem

Given a string `s`, return *the longest palindromic substring* in `s`.

<!-- more -->

**Example 1:**

```
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2:**

```
Input: s = "cbbd"
Output: "bb"
```

**Example 3:**

```
Input: s = "a"
Output: "a"
```

**Example 4:**

```
Input: s = "ac"
Output: "a"
```

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consist of only digits and English letters (lower-case and/or upper-case).

------

## Analysis

&emsp;&emsp;这道题目给定一个字符串，要求在这个字符串中找到最长的回文子串。字符串中找回文子串这类题目是非常典型的DP类题目，思考方向直接就往DP靠了。题目要求的是子串，实际上我们只需要知道起点和终点。于是直接定义转移方程`dp[i][j]`为字符串中位置`[i,j]`之间是否是回文串。接下来我们就看转移方程：

1. 当长度为1的时候，是回文串，所以`dp[i][i] = 1`；
2. 当长度大于2的时候，如果`dp[i + 1][j - 1] = 1`，且`s[i] == s[j]`，则说明`dp[i][j] = 1`；
3. 当长度为2的时候，条件2的判断就失效，所以要特殊处理这种情况；

------

## Solution

&emsp;&emsp;嵌套循环是少不了的，每个位置作为起点，每个位置作为终点都需要考虑一遍，然后每当我们找到一个回文串，维护一个left和len，最后就能通过这两个信息找到子串了。

------

## Code

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        int size = s.size();
        if (size == 0) {
            return "";
        }
        int dp[size][size], left = 0, len = 1;
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < size; j++) {
                dp[i][j] = 0;
            }
        }
        for (int i = 0; i < size; i++) {
            dp[i][i] = 1;
            for (int j = 0; j < i; j++) {
                dp[j][i] = (s[i] == s[j] && (i - j < 2 || dp[j + 1][i - 1]));
                if (dp[j][i] && len < i - j + 1) {
                    len = i - j + 1;
                    left = j;
                }
            }
        }
        return s.substr(left, len);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较经典的DP题目，如果熟悉DP的话对这类题目应该是秒解的。这道题这道题目的分享到这里，谢谢您的支持！
