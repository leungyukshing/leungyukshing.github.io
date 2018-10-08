---
title: LeetCode解题报告（十二）-- 7. Reverse Integer
tags:
  - LeetCode
mathjax: true
abbrlink: 37940
date: 2018-09-24 19:35:19
---
## Problem
Given a 32-bit signed integer, reverse digits of an integer.

**Example 1**:
```
Input: 123
Output: 321
```

**Example 2**:
```
Input: -123
Output: -321
```

**Example 3**:
```
Input: 120
Output: 21
```

**Note**:
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $[−2^{31},  2^{31} − 1]$. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.
<!-- more -->

## Analysis
&emsp;&emsp;这道题的考点是倒转数字。只看题目的话会觉得很容易，只需要使用每位取余的方法就可以完成。对于负数而言，额外增加一个变量记录即可。但是提交后发现，题目的测例包含了一些倒转后越界的数字，这个才是本题的难点。由于无法存储这个越界的数，自然我们也无法使用这个数与**界**去做比较。这里用到了比较巧妙地一点，我们可以减少一位分别进行比较，即**个位**与**其余位**分别比较，这样就可以判断出是否越界了。

## Solution
&emsp;&emsp;由于我们计算的方法是取余然后与前面相加，因此，我们需要在相加前做越界的判断，如果前面部分`result`大于`(2147483647-当前位)/10`，就说明两部分相加将会越界，此时返回0。其余的就没什么难度了。

## Code
```C++
class Solution {
public:
    int reverse(int x) {
        bool isNeg = false;
        if (x < 0) {
            x = -x;
            isNeg = true;
        }

        int result = 0;
        while (x != 0) {
            int tmp = x % 10;
            if (result > (2147483647-tmp)/10) {
                return 0;
            }
            result = result*10 + tmp;

            x /= 10;
        }

        if (isNeg) {
            result = -result;
        }

        return result;
    }
};
```
**运行时间**：约16ms，超过52.26%的CPP代码。

## Summary
&emsp;&emsp;这道题的算法思路很简单，但是在判断越界的方面给了我新的思路。在完成这题后，可以说以后碰到判断越界的情况都有所了解了，这题的分析就到这里，谢谢！
