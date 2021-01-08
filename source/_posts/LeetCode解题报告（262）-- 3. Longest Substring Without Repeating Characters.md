---
title: LeetCode解题报告（262）-- 3. Longest Substring Without Repeating Characters
tags:
  - LeetCode
mathjax: true
date: 2021-01-09 01:18:37

---

## Problem

Given a string `s`, find the length of the **longest substring** without repeating characters.

<!-- more -->

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

**Example 4:**

```
Input: s = ""
Output: 0
```

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.

------

## Analysis

&emsp;&emsp;题目给出一串字符，要求找到一个最长的子串，这个子串里面的字符不能有重复。很明显这就是滑动窗口的题目了。而我们要做的就是保证这个窗口内没有重复的元素，方法是记录每个元素的下标，`right`往右走的时候去检查是否已经出现过，如果已经出现过而且下标在`(left, right]`中，说明有重复的元素，需要把`left`设置到这个下标。每次检查后维护一个区间长度的最大值即可。

------

## Solution

&emsp;&emsp;注意我们维护的`left`是区间前一个元素，区间并不包含`s[left]`这个字符，所以初始化的时候初始为-1。

------

## Code

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int res = 0, left = -1, n = s.size();
        unordered_map<int, int> m;
        for (int i = 0; i < n; ++i) {
            if (m.count(s[i]) && m[s[i]] > left) {
                left = m[s[i]];  
            }
            m[s[i]] = i;
            res = max(res, i - left);            
        }
        return res;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较经典的字符串题目，是通过map来解决元素重复的问题，同时也是一道滑动窗口的经典应用题目。这道题这道题目的分享到这里，谢谢您的支持！
