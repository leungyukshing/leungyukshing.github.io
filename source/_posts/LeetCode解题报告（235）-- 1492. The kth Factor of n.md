---
title: LeetCode解题报告（235）-- 1492. The kth Factor of n
tags:
  - LeetCode
mathjax: true
date: 2020-12-05 02:49:40

---

## Problem

Given two positive integers `n` and `k`.

A factor of an integer `n` is defined as an integer `i` where `n % i == 0`.

Consider a list of all factors of `n` sorted in **ascending order**, return *the* `kth` *factor* in this list or return **-1** if `n` has less than `k` factors.

<!-- more -->

**Example 1:**

```
Input: n = 12, k = 3
Output: 3
Explanation: Factors list is [1, 2, 3, 4, 6, 12], the 3rd factor is 3.
```

**Example 2:**

```
Input: n = 7, k = 2
Output: 7
Explanation: Factors list is [1, 7], the 2nd factor is 7.
```

**Example 3:**

```
Input: n = 4, k = 4
Output: -1
Explanation: Factors list is [1, 2, 4], there is only 3 factors. We should return -1.
```

**Example 4:**

```
Input: n = 1, k = 1
Output: 1
Explanation: Factors list is [1], the 1st factor is 1.
```

**Example 5:**

```
Input: n = 1000, k = 3
Output: 4
Explanation: Factors list is [1, 2, 4, 5, 8, 10, 20, 25, 40, 50, 100, 125, 200, 250, 500, 1000].
```

**Constraints:**

- `1 <= k <= n <= 1000`

------

## Analysis

&emsp;&emsp;这道题比较简单，要求返回数字`n`的第`k`个因子（从小到大排列）。思路也非常直接，因为`n`的因子的范围必定是$[1, n]$，所以在这个区间内遍历，找到因子后存入数组，最后选取第`k`个就可以了。

&emsp;&emsp;在上面的思路的基础上可以简单做些优化。比如我们不需要把所有的因子都存下来，我们只需要计录下当前是第几个因子即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int kthFactor(int n, int k) {
        vector<int> factors;
        for (int i = 1; i <= n; i++) {
            if (n % i == 0) {
                factors.push_back(i);
            }
        }
        
        if (k <= factors.size()) {
            return factors[k - 1];
        }
        return -1;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目并没有很难，在这里仅做简单总结。这道题目的分享到这里，谢谢您的支持！
