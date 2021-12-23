---
title: LeetCode解题报告（440）-- 1088. Confusing Number II
mathjax: true
date: 2021-12-23 14:05:28
---

## Problem

A **confusing number** is a number that when rotated `180` degrees becomes a different number with **each digit valid**.

We can rotate digits of a number by `180` degrees to form new digits.

- When `0`, `1`, `6`, `8`, and `9` are rotated `180` degrees, they become `0`, `1`, `9`, `8`, and `6` respectively.
- When `2`, `3`, `4`, `5`, and `7` are rotated `180` degrees, they become **invalid**.

Note that after rotating a number, we can ignore leading zeros.

- For example, after rotating `8000`, we have `0008` which is considered as just `8`.

Given an integer `n`, return *the number of **confusing numbers** in the inclusive range* `[1, n]`.

<!-- more -->

**Example 1:**

```
Input: n = 20
Output: 6
Explanation: The confusing numbers are [6,9,10,16,18,19].
6 converts to 9.
9 converts to 6.
10 converts to 01 which is just 1.
16 converts to 91.
18 converts to 81.
19 converts to 61.
```

**Example 2:**

```
Input: n = 100
Output: 19
Explanation: The confusing numbers are [6,9,10,16,18,19,60,61,66,68,80,81,86,89,90,91,98,99,100].
```

**Constraints:**

- `1 <= n <= 109`

------

## Analysis

&emsp;&emsp;题目给出了confusing number的定义，是把数字整体翻转180度后，合法且与原来的数字不相等的数。这道题目要做的是返回`n`以内confusing number的数量。因为confusing number只能由5个可以反转180度后还是合法的数字组成，所以这里我们可以用递归的方法进行穷举，在穷举的过程中需要验证两个东西：

1. 数字是否是confusing number，虽然组成数字的每一个数字都是可以翻转，但是可能整体翻转180度后与原有数字相同，这种情况要排除在外；
2. 数字是否小于等于`n`，这个是题目的限制。

&emsp;&emsp;然后用一个全局的变量去统计符合要求的confusing number数量即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int confusingNumberII(int n) {
        unordered_map<int, int> m{{0, 0}, {1, 1}, {6, 9}, {8, 8}, {9, 6}};
        int result = 0;
        helper(n, 0, m, result);
        return result;
    }
private:
    void helper(int n, long cur, unordered_map<int, int> &m, int &result) {
        if (isConfusingNumber(cur, m)) {
            ++result;
        }
        
        for (auto &[k, v]: m) {
            if (cur * 10 + k <= n && cur * 10 + k != 0) {
                helper(n, cur * 10 + k, m, result);
            }
        }
    }
    
    bool isConfusingNumber(long cur, unordered_map<int, int> &m) {
        long temp = cur, newNum = 0;
        while (cur) {
            newNum = newNum * 10 + m[cur % 10];
            cur /= 10;
        }
        return temp != newNum;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是一个系列题，第一题就是判断是否confusing number，恰好这这道题目的一个子方法。对于返回`n`以内符合某种特殊要求的数字个数，一般采用构造法，即通过构造得到符合要求的数字，再限定范围。这道题目的分享到这里，感谢你的支持！
