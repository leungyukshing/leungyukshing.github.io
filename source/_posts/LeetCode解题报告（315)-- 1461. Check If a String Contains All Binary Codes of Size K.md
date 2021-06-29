---
title: >-
  LeetCode解题报告（315)-- 1461. Check If a String Contains All Binary Codes of Size
  K
tags:
  - LeetCode
mathjax: true
abbrlink: 24851
date: 2021-03-12 22:14:39
---

## Problem

Given a binary string `s` and an integer `k`.

Return *True* if every binary code of length `k` is a substring of `s`. Otherwise, return *False*.

<!-- more -->

**Example 1:**

```
Input: s = "00110110", k = 2
Output: true
Explanation: The binary codes of length 2 are "00", "01", "10" and "11". They can be all found as substrings at indicies 0, 1, 3 and 2 respectively.
```

**Example 2:**

```
Input: s = "00110", k = 2
Output: true
```

**Example 3:**

```
Input: s = "0110", k = 1
Output: true
Explanation: The binary codes of length 1 are "0" and "1", it is clear that both exist as a substring. 
```

**Example 4:**

```
Input: s = "0110", k = 2
Output: false
Explanation: The binary code "00" is of length 2 and doesn't exist in the array.
```

**Example 5:**

```
Input: s = "0000000001011100", k = 4
Output: false
```

**Constraints:**

- `1 <= s.length <= 5 * 10^5`
- `s` consists of 0's and 1's only.
- `1 <= k <= 20`

------

## Analysis

&emsp;&emsp;题目给了一个字符串`s`，还有一个数值`k`，问长度为`k`的二进制串是否都是`s` 的子串。这道题目要求解并不困难，最暴力的方法是把长度为`k`的二进制串都求出来，然后一个一个在`s`中进行对比，如果有一个不符合则return false，如果全部都能在`s`中找到，则return true。

&emsp;&emsp;但是这种做法有性能的问题。把长度为`k`的二进制串都算出来本身就要有一定的计算量，同时，还需要在`s`中寻找是否有匹配的子串，这种`find()`操作在性能上是很低的。我们能否有一种更加高效的做法呢？

&emsp;&emsp; 不妨逆向思考，我们不在`s`中找匹配的子串，而是从`s`中找出所有满足长度的子串，看看这些子串是否就是所有长度为`k`的二进制串。因为长度为`k`的二进制串的数量是$2^k$个，所以我们只需要在`s` 中提取出所有长度为`k`的字符串，放到一个集合中，如果集合中元素的个数是$2^k$，说明能够构成；否则不能。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    bool hasAllCodes(string s, int k) {
        unordered_set<string> dict;
        for (int i = 0; i + k <= s.size(); i++) {
            string temp = s.substr(i, k);
            dict.insert(temp);
        }
        
        if (dict.size() == pow(2, k)) {
            return true;
        }
        return false;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目本身难度不大，是借助了set这个数据结构做了一些优化。这道题目的分享到这里，感谢你的支持！
