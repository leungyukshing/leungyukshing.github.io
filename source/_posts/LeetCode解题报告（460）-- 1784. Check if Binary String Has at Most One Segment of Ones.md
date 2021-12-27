---
title: LeetCode解题报告（460）-- 1784. Check if Binary String Has at Most One Segment of Ones
mathjax: true
date: 2021-12-26 22:08:12
---

## Problem

Given a binary string `s` **without leading zeros**, return `true` *if* `s` *contains **at most one contiguous segment of ones***. Otherwise, return `false`.

<!-- more -->

**Example 1:**

```
Input: s = "1001"
Output: false
Explanation: The ones do not form a contiguous segment.
```

**Example 2:**

```
Input: s = "110"
Output: true
```

**Constraints:**

- `1 <= s.length <= 100`
- `s[i]` is either `'0'` or `'1'`.
- `s[0]` is `'1'`.

---

## Analysis

&emsp;&emsp;这道题目最大的难点就是理解题意，很无语。题目说只能有一个连续1的segment，而且给出的字符串又是以1开头的，所以其实意思是说，除了开头的连续1之外，后面不能再出现1。

&emsp;&emsp;那么很简单的判断方法就是判断是否存在`01`，存在`01`说明除了开头的1之外，中间间隔了至少1个0，然后后面又出现1了。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    bool checkOnesSegment(string s) {
        return s.find("01") == string::npos;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目感觉挺有意思，记录一下。这道题目的分享到这里，感谢你的支持！
