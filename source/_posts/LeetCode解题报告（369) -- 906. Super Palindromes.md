---
title: LeetCode解题报告（369) -- 906. Super Palindromes
tags:
  - LeetCode
mathjax: true
date: 2021-05-12 13:15:48

---

## Problem

Let's say a positive integer is a **super-palindrome** if it is a palindrome, and it is also the square of a palindrome.

Given two positive integers `left` and `right` represented as strings, return *the number of **super-palindromes** integers in the inclusive range* `[left, right]`.

<!-- more -->

**Example 1:**

```
Input: left = "4", right = "1000"
Output: 4
Explanation: 4, 9, 121, and 484 are superpalindromes.
Note that 676 is not a superpalindrome: 26 * 26 = 676, but 26 is not a palindrome.
```

**Example 2:**

```
Input: left = "1", right = "2"
Output: 1
```



**Constraints:**

- `1 <= left.length, right.length <= 18`
- `left` and `right` consist of only digits.
- `left` and `right` cannot have leading zeros.
- `left` and `right` represent integers in the range `[1, 1018]`.
- `left` is less than or equal to `right`.

------

## Analysis

&emsp;&emsp;题目要求找到在某个区间范围内的超级回文数的个数。超级回文数的定义是：

1. 它本身是一个回文数；
2. 同时它是某个回文数的平方。

&emsp;&emsp;在处理回文数的题目中，很常用的方法是构造法。我们使用递归去不断构造出回文数，从中间开始，每次递归都往两边各增加一位，构造完之后再判断一下是否在规定的范围内以及它的平方数是否是回文数即可。

------

## Solution

&emsp;&emsp;特别注意要判断首位是否为0，如果是0则不判断，但是要继续递归。

------

## Code

```c++
class Solution {
public:
    int superpalindromesInRange(string L, string R) {
        int res = 0;
        long left = stol(L), right = stol(R);
        helper("", left, right, res);
        for (char c = '0'; c <= '9'; ++c) {
            helper(string(1, c), left, right, res);
        }
        return res;
    }
    void helper(string cur, long& left, long& right, int& res) {
        if (cur.size() > 9) return;
        if (!cur.empty() && cur[0] != '0') {
            long num = stol(cur);
            num *= num;
            if (num > right) return;
            if (num >= left && isPalindrome(to_string(num))) ++res;
         }
         for (char c = '0'; c <= '9'; ++c) {
            helper(string(1, c) + cur + string(1, c), left, right, res);
        }
    }
    bool isPalindrome(string str) {
        int left = 0, right = (int)str.size() - 1;
        while (left < right) {
            if (str[left++] != str[right--]) return false;
        }
        return true;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是回文数中比较复杂的题目，基本思想还是构造出回文数，然后求平方数后，再判断是否是回文数。这道题目的分享到这里，感谢你的支持！
