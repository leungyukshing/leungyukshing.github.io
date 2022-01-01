---
title: LeetCode解题报告（488）-- 1910. Remove All Occurrences of a Substring
mathjax: true
date: 2022-01-01 18:41:51
---

## Problem

Given two strings `s` and `part`, perform the following operation on `s` until **all** occurrences of the substring `part` are removed:

- Find the **leftmost** occurrence of the substring `part` and **remove** it from `s`.

Return `s` *after removing all occurrences of* `part`.

A **substring** is a contiguous sequence of characters in a string.

<!-- more -->

**Example 1:**

```
Input: s = "daabcbaabcbc", part = "abc"
Output: "dab"
Explanation: The following operations are done:
- s = "daabcbaabcbc", remove "abc" starting at index 2, so s = "dabaabcbc".
- s = "dabaabcbc", remove "abc" starting at index 4, so s = "dababc".
- s = "dababc", remove "abc" starting at index 3, so s = "dab".
Now s has no occurrences of "abc".
```

**Example 2:**

```
Input: s = "axxxxyyyyb", part = "xy"
Output: "ab"
Explanation: The following operations are done:
- s = "axxxxyyyyb", remove "xy" starting at index 4 so s = "axxxyyyb".
- s = "axxxyyyb", remove "xy" starting at index 3 so s = "axxyyb".
- s = "axxyyb", remove "xy" starting at index 2 so s = "axyb".
- s = "axyb", remove "xy" starting at index 1 so s = "ab".
Now s has no occurrences of "xy".
```



**Constraints:**

- `1 <= s.length <= 1000`
- `1 <= part.length <= 1000`
- `s` and `part` consists of lowercase English letters.

---

## Analysis

&emsp;&emsp;这道题目给出一个字符串`s`以及一个模式`part`，要求当字符串存在子串和`part`一样时，就消除掉，问最后剩下的部分是什么。这道题目消除连续`k`个相同的字符串是一个道理，有两个下标实现一个类似stack的逻辑。下标`i`为快下标，用来遍历`s`，另一个下标`j`为慢下标，用来维护剩余的字符串到哪里了。每次检查`substr(j - m, m)`是否和`part`相等，`m`是`part`的长度，如果相等则回退`j`。最后`j`指向的位置就是结果的位置。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    string removeOccurrences(string s, string part) {
        string x = s;
        int n = s.size(), m = part.size(), j = 0;
        for (int i = 0; i < n; ++i) {
            x[j++] = s[i];
            if (j >= m && x.substr(j - m, m) == part) {
                j -= m;
            }
        }
        return x.substr(0, j);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目和之前消除连续`k`个字符原理是一样的，只不过这道题目消除的是子串，消除的逻辑不同。之前的题目是通过统计数字来消除，而这道题目是通过对比子串消除。。这道题目的分享到这里，感谢你的支持！

