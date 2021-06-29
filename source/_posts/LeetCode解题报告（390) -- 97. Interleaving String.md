---
title: LeetCode解题报告（390) -- 97. Interleaving String
tags:
  - LeetCode
mathjax: true
abbrlink: 11899
date: 2021-06-10 23:48:27
---

## Problem

Given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an **interleaving** of `s1` and `s2`.

An **interleaving** of two strings `s` and `t` is a configuration where they are divided into **non-empty** substrings such that:

- `s = s1 + s2 + ... + sn`
- `t = t1 + t2 + ... + tm`
- `|n - m| <= 1`
- The **interleaving** is `s1 + t1 + s2 + t2 + s3 + t3 + ...` or `t1 + s1 + t2 + s2 + t3 + s3 + ...`

**Note:** `a + b` is the concatenation of strings `a` and `b`.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg)

```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true
```

**Example 2:**

```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
Output: false
```

**Example 3:**

```
Input: s1 = "", s2 = "", s3 = ""
Output: true
```



**Constraints:**

- `0 <= s1.length, s2.length <= 100`
- `0 <= s3.length <= 200`
- `s1`, `s2`, and `s3` consist of lowercase English letters.

 

**Follow up:** Could you solve it using only `O(s2.length)` additional memory space?

------

## Analysis

&emsp;&emsp;题目给出两个字符串`s1`和`s2`，同时还给出了一个`s3`，问能否通过交织`s1`和`s2`得到`s3`？实际上就是在`s3`中找到哪些字符是从`s1`来的，哪些是从`s2`来的。这类问题直接就是往dp方向考虑。

&emsp;&emsp;定义`dp[i][j]`为由`s1`前`i`个字符和`s2`前`j`个字符能否匹配`s3`的前`i + j`个字符。首先考虑边界情况，即只有`s1`或只有`s2`的case，转移方程为：

- `dp[i][0] = dp[i - 1][0] && s1[i - 1] == s3[i - 1]`；
- `dp[0][j] = dp[0][j - 1] && s2[j - 1] == s3[j - 1]`

&emsp;&emsp;然后再来看中间状态的转移：

- `dp[i][j] = (dp[i - 1][j] && s1[i - 1] == s3[i + j - 1]) || (dp[i][j - 1] && s2[j - 1] == s3[i + j - 1])`；

&emsp;&emsp;可以看出`s3[i + j - 1]`是由两部分共同决定的，这个位置的字符既可以是由`s1`提供的`s1[i - 1]`，也可以是由`s2`提供的`s2[j - 1]`。

------

## Solution

&emsp;&emsp;这里还有一个技巧是可以在一开始先通过长度过滤掉一部分不符合要求的case。

------

## Code

```c++
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int size1 = s1.size(), size2 = s2.size(), size3 = s3.size();
        if (size1 + size2 != size3) {
            return false;
        }
        vector<vector<bool>> dp(size1 + 1, vector<bool>(size2 + 1, false));
        dp[0][0] = true;
        for (int i = 1; i <= size1; i++) {
            dp[i][0] = dp[i - 1][0] && (s1[i - 1] == s3[i - 1]);
        }
        
        for (int j = 1; j <= size2; j++) {
            dp[0][j] = dp[0][j - 1] && (s2[j - 1] == s3[j - 1]);
        }
        
        for (int i = 1; i <= size1; i++) {
            for (int j = 1; j <= size2; j++) {
                dp[i][j] = (dp[i - 1][j] && (s1[i - 1] == s3[i + j - 1])) || (dp[i][j - 1] && (s2[j - 1] == s3[i + j - 1]));
            }
        }
        return dp[size1][size2];
    }
};
```

------

## Summary

&emsp;&emsp;这道题是难度中等的字符串dp，字符串的dp了总体思路是比较清晰的，用一个二维dp，然后分别从两个字符串中取字符做对比。这道题目的分享到这里，感谢你的支持！
