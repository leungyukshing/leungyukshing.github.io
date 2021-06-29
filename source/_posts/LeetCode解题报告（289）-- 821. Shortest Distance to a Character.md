---
title: LeetCode解题报告（289）-- 821. Shortest Distance to a Character
tags:
  - LeetCode
mathjax: true
abbrlink: 58351
date: 2021-02-10 02:30:52
---

## Problem

Given a string `s` and a character `c` that occurs in `s`, return *an array of integers* `answer` *where* `answer.length == s.length` *and* `answer[i]` *is the **distance** from index* `i` *to the **closest** occurrence of character* `c` *in* `s`.

The **distance** between two indices `i` and `j` is `abs(i - j)`, where `abs` is the absolute value function.

<!-- more -->

**Example 1:**

```
Input: s = "loveleetcode", c = "e"
Output: [3,2,1,0,1,0,0,1,2,2,1,0]
Explanation: The character 'e' appears at indices 3, 5, 6, and 11 (0-indexed).
The closest occurrence of 'e' for index 0 is at index 3, so the distance is abs(0 - 3) = 3.
The closest occurrence of 'e' for index 1 is at index 3, so the distance is abs(1 - 3) = 3.
For index 4, there is a tie between the 'e' at index 3 and the 'e' at index 5, but the distance is still the same: abs(4 - 3) == abs(4 - 5) = 1.
The closest occurrence of 'e' for index 8 is at index 6, so the distance is abs(8 - 6) = 2.
```

**Example 2:**

```
Input: s = "aaab", c = "b"
Output: [3,2,1,0]
```

**Constraints:**

- `1 <= s.length <= 104`
- `s[i]` and `c` are lowercase English letters.
- It is guaranteed that `c` occurs at least once in `s`.

------

## Analysis

&emsp;&emsp;题目给出一个字符串以及一个字符`c`，问字符串上每个位置距离该字符的最短距离。在字符串中除了`c`以外，其他字符距离最近的`c`要么在它的左边，要么在它的右边，所以我们只需要从左往右遍历一遍，再从右往左遍历一遍，就能找到答案了。

&emsp;&emsp;从左往右遍历的时候，除了`c`以外，每个位置的距离比前一个`i - 1`递增1；同理，从右往左遍历时，除了`c`之外的位置的距离比前一个`i + 1 `递增1。第一遍计算的时候不需要取最小值，第二遍计算的时候需要和第一遍的结果进行比较，取极小值。

------

## Solution

&emsp;&emsp;这里初始化有个细节需要注意，因为是求最小值，所以初始化的时候把值设置为数组的长度即可。

------

## Code

```c++
class Solution {
public:
    vector<int> shortestToChar(string s, char c) {
        int size = s.size();
        vector<int> result(size, size);
        for (int i = 0; i < size; i++) {
            if (s[i] == c) {
                result[i] = 0;
            } else if (i > 0) {
                result[i] = result[i - 1] + 1;
            }
        }
        
        for (int i = size - 2; i >= 0; i--) {
            result[i] = min(result[i], result[i + 1] + 1);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，谢谢您的支持！
