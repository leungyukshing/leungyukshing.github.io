---
title: LeetCode解题报告（206）-- 1446. Consecutive Characters
tags:
  - LeetCode
mathjax: true
date: 2020-11-03 16:17:20

---

## Problem

Given a string `s`, the power of the string is the maximum length of a non-empty substring that contains only one unique character.

Return *the power* of the string.

<!-- more -->

**Example 1:**

```
Input: s = "leetcode"
Output: 2
Explanation: The substring "ee" is of length 2 with the character 'e' only.
```

**Example 2:**

```
Input: s = "abbcccddddeeeeedcba"
Output: 5
Explanation: The substring "eeeee" is of length 5 with the character 'e' only.
```

**Example 3:**

```
Input: s = "triplepillooooow"
Output: 5
```

**Example 4:**

```
Input: s = "hooraaaaaaaaaaay"
Output: 11
```

**Example 5:**

```
Input: s = "tourist"
Output: 1
```

**Constraints:**

- `1 <= s.length <= 500`
- `s` contains only lowercase English letters.

------

## Analysis

&emsp;&emsp;题目给出一个字符串，要求计算最长的相同字符字串长度。因为答案只需要长度，所以一次遍历就可以完成。遍历的时候和前一个字符对比是否相同，如果相同则长度+1，否则，长度从1开始重新计算，到最后维护一个最大的长度即可。

------

## Solution

&emsp;&emsp;可以用一个额外的char记录前一个字符，用于比较。记得在不同的时候，需要把长度重置为1。

------

## Code

```c++
class Solution {
public:
    int maxPower(string s) {
        int size = s.size();
        if (size == 0) {
            return 0;
        }
        char ch = s[0];
        int len = 1, result = 1;
        for (int i = 1; i < size; i++) {
            if (s[i] == ch) {
                len += 1;
            } else {
                ch = s[i];
                result = max(result, len);
                len = 1;
            }
        }
        result = max(result, len);
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目比较简单，但是在一些面试中也会作为热身题出现，所以在这里总结一下思路，以后碰到就可以秒解了。这道题目的分享到这里，谢谢！
