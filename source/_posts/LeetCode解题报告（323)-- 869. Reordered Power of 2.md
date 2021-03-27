---
title: LeetCode解题报告（323)-- 869. Reordered Power of 2
tags:
  - LeetCode
mathjax: true
date: 2021-03-26 02:03:14

---

## Problem

You are given an integer `n`. We reorder the digits in any order (including the original order) such that the leading digit is not zero.

Return `true` *if and only if we can do this so that the resulting number is a power of two*.

<!-- more -->

**Example 1:**

```
Input: n = 1
Output: true
```

**Example 2:**

```
Input: n = 10
Output: false
```

**Example 3:**

```
Input: n = 16
Output: true
```

**Example 4:**

```
Input: n = 24
Output: false
```

**Example 5:**

```
Input: n = 46
Output: true
```

**Constraints:**

- `1 <= n <= 109`

------

## Analysis

&emsp;&emsp;题目给出了一个十进制数`N`，问能不能通过改变数字的顺序，使得这个数是2的次幂。正常的思路是找这个数的排列组合，然后逐个校验是否2的次幂，但是这种做法效率不高。第一，如果这个数比较大，那么它的排列组合会很多；第二，因为不能以0为开头，所以当某个数包含非常多0的时候，实际上需要校验的组合是很少的。

&emsp;&emsp;为了解决上面两个问题，我们逆向思考，从2的次幂出发去解决这个问题。`[1, 1e+9]`的范围内，2的次幂是很少的，所以遍历这个效率是比较高。然后得到2的次幂后，把它（十进制）转换为字符串。然后把这个字符串进行排序，同时对题目给出的`N`的字符串形式进行排序，最后对比两个有序的字符串是否一致。如果一致，则说明是可以通过交换位置得到2的次幂的。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    bool reorderedPowerOf2(int N) {
        string num = to_string(N);
        sort(num.begin(), num.end());
        for (int i = 0; i < 31; i++) {
            string t = to_string(1 << i);
            sort(t.begin(), t.end());
            if (t == num) {
                return true;
            }
        }
        return false;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是属于字符串类型的题目，主要是依赖排序后的字符串对比来判断，同时还运用到了位运算符，求出2的次幂。这道题目的分享到这里，感谢你的支持！
