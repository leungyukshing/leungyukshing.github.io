---
title: LeetCode解题报告（292）-- 242. Valid Anagram
tags:
  - LeetCode
mathjax: true
abbrlink: 8704
date: 2021-02-14 11:54:08
---

## Problem

Given two strings *s* and *t* , write a function to determine if *t* is an anagram of *s*.

<!-- more -->

**Example 1:**

```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Example 2:**

```
Input: s = "rat", t = "car"
Output: false
```

**Note:**
You may assume the string contains only lowercase alphabets.

**Follow up:**
What if the inputs contain unicode characters? How would you adapt your solution to such case?

------

## Analysis

&emsp;&emsp;题目给出两个词语，要求判断这两个是否**同字母异序词**。和之前有一道题目非常类似，主要判断三个条件：

1. 长度是否相等；
2. 组成的元素是否相同；
3. 每个元素出现的次数是否相同。

------

## Solution

&emsp;&emsp;上面的三个条件中，第一个很容易判断，第二个和第三个就需要用一个数组来统计。

------

## Code

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int count[26] = {0};
        int size1 = s.size(), size2 = t.size();
        if (size1 != size2) {
            return false;
        }
        for (int i = 0; i < size1; i++) {
            ++count[s[i] - 'a'];
        }
        for (int i = 0; i < size2; i++) {
            if (--count[t[i] - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，谢谢您的支持！
