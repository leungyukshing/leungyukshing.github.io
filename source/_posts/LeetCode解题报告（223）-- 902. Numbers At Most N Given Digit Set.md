---
title: LeetCode解题报告（223）-- 902. Numbers At Most N Given Digit Set
tags:
  - LeetCode
mathjax: true
date: 2020-11-21 21:49:40

---

## Problem

Given an array of `digits`, you can write numbers using each `digits[i]` as many times as we want.  For example, if `digits = ['1','3','5']`, we may write numbers such as `'13'`, `'551'`, and `'1351315'`.

Return *the number of positive integers that can be generated* that are less than or equal to a given integer `n`.

<!-- more -->

**Example 1:**

```
Input: digits = ["1","3","5","7"], n = 100
Output: 20
Explanation: 
The 20 numbers that can be written are:
1, 3, 5, 7, 11, 13, 15, 17, 31, 33, 35, 37, 51, 53, 55, 57, 71, 73, 75, 77.
```

**Example 2:**

```
Input: digits = ["1","4","9"], n = 1000000000
Output: 29523
Explanation: 
We can write 3 one digit numbers, 9 two digit numbers, 27 three digit numbers,
81 four digit numbers, 243 five digit numbers, 729 six digit numbers,
2187 seven digit numbers, 6561 eight digit numbers, and 19683 nine digit numbers.
In total, this is 29523 integers that can be written using the digits array.
```

**Example 3:**

```
Input: digits = ["7"], n = 8
Output: 1
```

**Constraints:**

- `1 <= digits.length <= 9`
- `digits[i].length == 1`
- `digits[i]` is a digit from `'1'` to `'9'`.
- All the values in `digits` are **unique**.
- `1 <= n <= 109`

------

## Analysis

&emsp;&emsp;这是一道构成数字类的题目，题目给定了一个数组，里面是可以利用的数字（可以重复使用），同时给定了一个上界，要求使用题目给出的数字组成出不超过这个上界的数的数量。因为这道题目只需要计算数量，不要求把所有的结果都返回，所以在下面思考的时候也要把这个考虑进去，有可能能简化思路。

&emsp;&emsp;我们来看看题目本身，要组成比`n`小的数，其实有两种思路：一是位数比`n`小，只要位数小，怎么组成都不会超过`n`；二是位数相同的，这种情况比较复杂后面再分析。所以第一种其实是非常容易计算的，因为每一位所有数字都可以选，假设有4个数字可选，则`x`位数的组合就是$4^x$。如果`n`是一个`k`位数，那么位数比`n`小的数字的总数就是$4 + 4^2 + ... + 4^{k - 1}$。

&emsp;&emsp;接下来我们来着重分析位数相等的情况。从高位到低位看，分如下几种情况：

1. `n`的第`j`位比`digits[i]`大：后面的位置数字任意选择，$4^{k-j-1}$
2. `n`的第`j`位比`digits[i]`小：说明已经超过了`n`，后面的位置不需要判断了。
3. `n`的第`j`位和`digits[i]`相等：进入下一位的判断。

------

## Solution

&emsp;&emsp;按照上面的思路，使用两个循环进行判断，外循环遍历`n`的每个位置，内循环遍历可用的数字。特别注意，当`n `的某一位所有候选的数字都没有和它相等，则说明不需要进入下一位的判断了，所以可以直接return。

------

## Code

```c++
class Solution {
public:
    int atMostNGivenDigitSet(vector<string>& digits, int n) {
        int size = digits.size();
        string str = to_string(n);
        int str_size = str.size();
        
        int result = 0;
        // calculate all composition except the highest digit
        for (int i = 1; i < str_size; i++) {
            result += pow(size, i);
        }
        
        // calculate the highest digit
        for (int i = 0; i < str_size; i++) {
            bool hasSameDigit = false;
            for (int j = 0; j < size; j++) {
                if (digits[j][0] < str[i]) {
                    result += pow(size, str_size - i - 1);
                } else if (digits[j][0] == str[i]) {
                    hasSameDigit = true;
                    // no handle in this loop, will handle the next digit in the following loop
                } else {
                    break;
                }
            }
            if (!hasSameDigit) {
                return result;
            }
        }
        return result+1;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目虽然考虑起来会比较复杂，但原理还是数字大小比较，遵循的原则是从高到低。因为这道题目可选数字是可以重复使用的，一定程度上也降低了难度。这道题目的分享到这里，谢谢！
