---
title: LeetCode解题报告（123）-- 459. Repeated Substring Pattern
tags:
  - LeetCode
mathjax: true
date: 2020-09-03 18:59:11

---

## Problem

Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.

<!-- more -->

**Example 1:**

```
Input: "abab"
Output: True
Explanation: It's the substring "ab" twice.
```

**Example 2:**

```
Input: "aba"
Output: False
```

**Example 3:**

```
Input: "abcabcabcabc"
Output: True
Explanation: It's the substring "abc" four times. (And the substring "abcabc" twice.)
```

------

## Analysis

&emsp;&emsp;这道题给出了一个字符串，要求我们判断这个字符串能否通过重复其中的子串来构成。这道题目一开始思考的方向是字符串匹配，可能会考虑到类似于KMP的算法。但这里我从构造的方向考虑，思路变为**能不能找到某个子串能够构造出原字符串**。因为是重复构造，假设子串的长度为`x` ，原字符串的长度为`y`，那么必有`y % x = 0`，且`y / x >= 2`。所以我们可以从`x`下手，从`y/2`一直遍历到1。如果能被`y`整除，就计算出重复的次数，然后尝试构造出新的字符串，最后对比新字符串和原字符串是否相同。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        int size = s.size();
        for (int i = size / 2; i >= 1; i--) {
            if (size % i == 0) {
                int times = size / i;
                string temp = "";
                for (int j = 0; j < times; j++) {
                    temp += s.substr(0, i);
                }
                // cout << temp << endl;
                if (temp == s) {
                    return true;
                }
            }
        }
        return false;
    }
};
```

------

## Summary

&emsp;&emsp;字符串匹配类型的题目有时候也可以从构造的方向去解决，像这道题目就可以利用重复的特性去构造。这道题的分析到这里，谢谢！
