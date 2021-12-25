---
title: LeetCode解题报告（449）-- 552. Student Attendance Record II
mathjax: true
date: 2021-12-24 22:25:31
---

## Problem

An attendance record for a student can be represented as a string where each character signifies whether the student was absent, late, or present on that day. The record only contains the following three characters:

- `'A'`: Absent.
- `'L'`: Late.
- `'P'`: Present.

Any student is eligible for an attendance award if they meet **both** of the following criteria:

- The student was absent (`'A'`) for **strictly** fewer than 2 days **total**.
- The student was **never** late (`'L'`) for 3 or more **consecutive** days.

Given an integer `n`, return *the **number** of possible attendance records of length* `n` *that make a student eligible for an attendance award. The answer may be very large, so return it **modulo*** `109 + 7`.

<!-- more -->

**Example 1:**

```
Input: n = 2
Output: 8
Explanation: There are 8 records with length 2 that are eligible for an award:
"PP", "AP", "PA", "LP", "PL", "AL", "LA", "LL"
Only "AA" is not eligible because there are 2 absences (there need to be fewer than 2).
```

**Example 2:**

```
Input: n = 1
Output: 3
```

**Example 3:**

```
Input: n = 10101
Output: 183236316
```

**Constraints:**

- `1 <= n <= 105`

------

## Analysis

&emsp;&emsp;这道题目是系列题，第一个题目太简单了就不写题解了，主要就是给出一个字符串序列，要求判断是不是eligible for an attendance award。还是这道题目比较有意思，题目的规则和第一题一样，不同在于它是给出了一个`n`，问有多少种不同的组合。我们一看答案要取模，下意识就是dp了。状态很简单，就是`n`个状态。

&emsp;&emsp;定义三个dp数组，分别是以A结尾，以L结尾和以P结尾，那么答案就是三个的和。我们先来看转移方程。

+ 对`P[i]`来说，因为没有什么限制，所以`P[i] = P[i - 1] + A[i - 1] + L[i - 1]`；
+ 对`L[i]`来说，分成两部分，第一部分是当`i - 1`是`L`的话，因为不能连续三个L，所以`i - 2`是P或者A，应该是`P[i - 2] + A[i - 2]`；第二部分是当`i - 1`是A或P的话，直接就是`P[i - 1] + A[i - 1]`，所以整体就是`L[i] = P[i - 1] + A[i - 1] + P[i - 2] + A[i - 2]`；
+ 对`A[i]`来说是最难的，因为我们要之前之前出现过几次A，如果当前是以A结尾，说明之前是不能出现A的，我们先定义`P1[i]`为前i个以P结尾但不包含A的数量；定义`L1[i]`为前i个以L结尾但不包含L的数量，这两个的转移方程很好计算，可以参考上面两个的计算，`P1[i] = P1[i - 1] + L1[i - 1]`，`L1[i] = P1[i - 1] + P1[i - 2]`，然后`A[i] = P1[i - 1] + L1[i - 1]`，代入上面两个式子化简得到`A[i] = A[i - 1] + A[i - 2] + A[i - 3]`。

&emsp;&emsp;至此，状态转移方程就写完了，可以直接写代码了。

## Solution

&emsp;&emsp;这里的base case需要额外处理，三个dp数组的base case不一样，我们只需要考虑遍历的时候下标什么时候不合法，把这些case处理就可以了。

------

## Code

```c++
class Solution {
public:
    int checkRecord(int n) {
        const int M = 1e9 + 7;
        vector<int> P(n), A(n), L(n);
        
        P[0] = 1, A[0] = 1, L[0] = 1;
        if (n > 1) {
            L[1] = 3;
            A[1] = 2;
        }
        if (n > 2) {
            A[2] = 4;
        }
        
        for (int i = 1; i < n; ++i) {
            P[i] = ((P[i - 1] + A[i - 1]) % M + L[i - 1]) % M;
            if (i > 1) {
                L[i] = (((A[i - 1] + P[i - 1]) % M + P[i - 2]) % M + A[i - 2]) % M;
            }
            if (i > 2) {
                A[i] = ((A[i - 1] + A[i - 2]) % M + A[i - 3]) % M;
            }
        }
        
        return ((P[n - 1] + A[n - 1]) % M + L[n - 1]) % M;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是dp中算比较困难的，主要难点在于`A[i]`的化简。这道题目的分享到这里，感谢你的支持！
