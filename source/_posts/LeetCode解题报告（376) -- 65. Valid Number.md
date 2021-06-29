---
title: LeetCode解题报告（376) -- 65. Valid Number
tags:
  - LeetCode
mathjax: true
abbrlink: 58796
date: 2021-05-19 12:27:58
---

## Problem

A **valid number** can be split up into these components (in order):

1. A **decimal number** or an **integer**.
2. (Optional) An `'e'` or `'E'`, followed by an **integer**.

A **decimal number** can be split up into these components (in order):

1. (Optional) A sign character (either `'+'` or `'-'`).
2. One of the following formats:
   1. At least one digit, followed by a dot `'.'`.
   2. At least one digit, followed by a dot `'.'`, followed by at least one digit.
   3. A dot `'.'`, followed by at least one digit.

An **integer** can be split up into these components (in order):

1. (Optional) A sign character (either `'+'` or `'-'`).
2. At least one digit.

For example, all the following are valid numbers: `["2", "0089", "-0.1", "+3.14", "4.", "-.9", "2e10", "-90E3", "3e+7", "+6e-1", "53.5e93", "-123.456e789"]`, while the following are not valid numbers: `["abc", "1a", "1e", "e3", "99e2.5", "--6", "-+3", "95a54e53"]`.

Given a string `s`, return `true` *if* `s` *is a **valid number***.

<!-- more -->

**Example 1:**

```
Input: s = "0"
Output: true
```

**Example 2:**

```
Input: s = "e"
Output: false
```

**Example 3:**

```
Input: s = "."
Output: false
```

**Example 4:**

```
Input: s = ".1"
Output: true
```



**Constraints:**

- `1 <= s.length <= 20`
- `s` consists of only English letters (both uppercase and lowercase), digits (`0-9`), plus `'+'`, minus `'-'`, or dot `'.'`.

------

## Analysis

&emsp;&emsp;这道题目要求我们验证给出的字符串是否一个合法的数字，这种题目考查的主要是各种case的判断。这道题目的情况实在是有点多，而且test case很复杂，直接参考[LeetCode Valid Number](https://www.cnblogs.com/grandyang/p/4084408.html)。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    bool isNumber(string s) {
        int len = s.size();
        int left = 0, right = len - 1;
        bool eExisted = false;
        bool dotExisted = false;
        bool digitExisited = false;
        // Delete spaces in the front and end of string
        while (s[left] == ' ') ++left;
        while (s[right] == ' ') --right;
        // If only have one char and not digit, return false
        if (left >= right && (s[left] < '0' || s[left] > '9')) return false;
        //Process the first char
        if (s[left] == '.') dotExisted = true;
        else if (s[left] >= '0' && s[left] <= '9') digitExisited = true;
        else if (s[left] != '+' && s[left] != '-') return false;
        // Process the middle chars
        for (int i = left + 1; i <= right - 1; ++i) {
            if (s[i] >= '0' && s[i] <= '9') digitExisited = true;
            else if (s[i] == 'e' || s[i] == 'E') { // e/E cannot follow +/-, must follow a digit
                if (!eExisted && s[i - 1] != '+' && s[i - 1] != '-' && digitExisited) eExisted = true;
                else return false;
            } else if (s[i] == '+' || s[i] == '-') { // +/- can only follow e/E
                if (s[i - 1] != 'e' && s[i - 1] != 'E') return false;                
            } else if (s[i] == '.') { // dot can only occur once and cannot occur after e/E
                if (!dotExisted && !eExisted) dotExisted = true;
                else return false;
            } else return false;
        }
        // Process the last char, it can only be digit or dot, when is dot, there should be no dot and e/E before and must follow a digit
        if (s[right] >= '0' && s[right] <= '9') return true;
        else if (s[right] == '.' && !dotExisted && !eExisted && digitExisited) return true;
        else return false;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，感谢你的支持！
