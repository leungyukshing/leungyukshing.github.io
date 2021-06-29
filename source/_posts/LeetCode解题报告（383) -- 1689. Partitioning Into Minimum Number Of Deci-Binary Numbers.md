---
title: >-
  LeetCode解题报告（383) -- 1689. Partitioning Into Minimum Number Of Deci-Binary
  Numbers
tags:
  - LeetCode
mathjax: true
abbrlink: 53211
date: 2021-06-06 12:16:28
---

## Problem

A decimal number is called **deci-binary** if each of its digits is either `0` or `1` without any leading zeros. For example, `101` and `1100` are **deci-binary**, while `112` and `3001` are not.

Given a string `n` that represents a positive decimal integer, return *the **minimum** number of positive **deci-binary** numbers needed so that they sum up to* `n`*.*

<!-- more -->

**Example 1:**

```
Input: n = "32"
Output: 3
Explanation: 10 + 11 + 11 = 32
```

**Example 2:**

```
Input: n = "82734"
Output: 8
```

**Example 3:**

```
Input: n = "27346209830709182346"
Output: 9
```



**Constraints:**

- `1 <= n.length <= 105`
- `n` consists of only digits.
- `n` does not contain any leading zeros and represents a positive integer.

------

## Analysis

&emsp;&emsp;这道题题目给出一个数字，要求用01组成的十进制数去构成，问需要多少个这种数。除了最高位外，每一个位置我们都可以根据需要选择0或者1，所以我们只需要关注最大的那个位即可。比如`1234`，我们只需要关注最后一位即可，因为最后一位满足了，前面的总能找到方法满足。最后一位是4，表示至少要有4个数，而且这四个数最后一位都是1，这样才能构成。

&emsp;&emsp;所以这道题目实际上是一道智力题，我们只需要找到数字中最大的位，那个值就是所需要的**deci-binary**的数量。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int minPartitions(string n) {
        char ch = '0';
        int size = n.size();
        for (int i = 0; i < size; i++) {
            ch = (char)max(ch, n[i]);
        }
        return ch - '0';
    }
};
```

------

## Summary

&emsp;&emsp;这类智力题对coding考察并不多，主要还是要多观察。这道题目的分享到这里，感谢你的支持！
