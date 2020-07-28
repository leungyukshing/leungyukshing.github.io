---
title: LeetCode解题报告（109）-- 258. Add Digits
tags:
  - LeetCode
date: 2020-07-28 10:48:29
mathjax: true


---

## Problem

Given a non-negative integer `num`, repeatedly add all its digits until the result has only one digit.

<!-- more -->

**Example:**

```
Input: 38
Output: 2 
Explanation: The process is like: 3 + 8 = 11, 1 + 1 = 2. 
             Since 2 has only one digit, return it.
```

**Follow up:**
Could you do it without any loop/recursion in O(1) runtime?

------

## Analysis

&emsp;&emsp;这道题目的意思非常简单，给出一个数字，要求我们把每一位的数字相加，直至只有一位。最简单的做法就是用一个while循环判断是否只剩一位，但是在follow up那里题目给出了一个挑战：能不能在$O(1)$时间内完成？这就要求我们不能跟着题目计算的思路去做，而是要**找规律**。一开始这个规律可能不太好下手，我们可以先从结果看，由于最后必须要加到只剩下一位，所以最后的结果一定就是在$[0, 9]$这个区间内。把任意的数值转换到一个指定的区域内，这个很容易联想到取余的操作。

&emsp;&emsp;然后我们列几个数字来验证一下是否正确，直接用输入的数字对9取余，发现除了9的倍数之外，都是符合规律的（如`38 % 9 = 2`）。但是对于9来说，直接取余则会为0。为了兼容这种情况，我们在计算的时候转换一下就可以了，先对`num - 1`，然后取余，最后再`+1`，这样就能够兼容9的倍数的情况了。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int addDigits(int num) {
        return (num - 1) % 9 + 1;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目虽然用循环的方法也能做出来，但是也有巧解的方法。对于答案在固定区间的题目，可以优先考虑取余。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
