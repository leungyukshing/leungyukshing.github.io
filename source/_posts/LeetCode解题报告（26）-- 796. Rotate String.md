---
title: LeetCode解题报告（26）-- 796. Rotate String
tags:
  - LeetCode
abbrlink: 1566
date: 2018-10-30 16:22:26
---
## Problem
We are given two strings, `A` and `B`.

A *shift* on `A` consists of taking string A and moving the leftmost character to the rightmost position. For example, if `A = 'abcde'`, then it will be `'bcdea'` after one shift on `A`. Return `True` if and only if `A` can become `B` after some number of shifts on `A`.
<!-- more -->

**Example 1**:
```
Input: A = 'abcde', B = 'cdeab'
Output: true
```

**Example 2**:
```
Input: A = 'abcde', B = 'abced'
Output: false
```

## Analysis
&emsp;&emsp;这道题涉及到的是字符串移位的操作。难点在于这种移位是循环的移位，所以我们要考虑到尾部的赋值问题。基本的思路是：对原字符串`A`，输出结果`B`，`B`先从头`i = 0`开始，一直赋值到`i = size - shift`，然后再用另一个变量`j = 0`从A的头开始接着赋值给`B[i]`，直到`j = shift`。

## Solution
&emsp;&emsp;使用两个循环即可实现上述操作了，注意下下标的边界问题和空字符串问题即可。

## Code
```C++
class Solution {
public:
    bool rotateString(string A, string B) {
        if (A.size() == 0 && B.size() == 0)
            return true;
        int size = A.size();
        for (int shift = 0; shift < size; shift++) {
            string str = rotate(A, shift);
            if (str == B) {
                return true;
            }
        }
        return false;
    }
    string rotate(string A, int shift) {
        string output = A;
        int size = A.size();
        int index = 0;
        for (int i = 0; i < size; i++) {
            if (i < size - shift) {
                output[i] = A[i + shift];
            }
            else {
                output[i] = A[index];
                index++;
            }
        }
        return output;
    }
};
```
**运行时间：**约0ms，超过100%的CPP代码。
## Summary
&emsp;&emsp;这道题实现起来难度不大，只要自己在纸上演算几次就能找到规律。最近刚好在写DES算法，其中也用到了循环左移的操作，因此这个算法在数据加密的领域非常常见，因此我认为值得与大家分享。本题的分析到这里，谢谢您的支持！
