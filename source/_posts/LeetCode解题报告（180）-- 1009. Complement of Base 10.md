---
title: LeetCode解题报告（180）-- 1009. Complement of Base 10 Integer
tags:
  - LeetCode
mathjax: true
date: 2020-10-05 15:12:11

---

## Problem

Every non-negative integer `N` has a binary representation.  For example, `5` can be represented as `"101"` in binary, `11` as `"1011"` in binary, and so on.  Note that except for `N = 0`, there are no leading zeroes in any binary representation.

The *complement* of a binary representation is the number in binary you get when changing every `1` to a `0` and `0` to a `1`.  For example, the complement of `"101"` in binary is `"010"` in binary.

For a given number `N` in base-10, return the complement of it's binary representation as a base-10 integer.

<!-- more -->

**Example 1:**

```
Input: 5
Output: 2
Explanation: 5 is "101" in binary, with complement "010" in binary, which is 2 in base-10.
```

**Example 2:**

```
Input: 7
Output: 0
Explanation: 7 is "111" in binary, with complement "000" in binary, which is 0 in base-10.
```

**Example 3:**

```
Input: 10
Output: 5
Explanation: 10 is "1010" in binary, with complement "0101" in binary, which is 5 in base-10.
```

**Note:**

1. `0 <= N < 10^9`
2. This question is the same as 476: https://leetcode.com/problems/number-complement/

------

## Analysis

&emsp;&emsp;这道题目是求十进制数的反码，要求输入的格式也是十进制。这套题目在算法上是不难的，求反码的方法只有一个，就是按位取反，这里没有文章可做。这道题目有意思的地方在coding的地方。

&emsp;&emsp;第一次做这个题的时候，我的方法是比较蠢且繁琐的。先把题目给出的十进制数专为二进制的字符串，然后对字符串进行遍历，逐位取反码，然后把字符串再转换回十进制数。这里存在着大量的循环遍历操作，虽然对整体的性能影响不大，但是实现起来非常不美观。

&emsp;&emsp;后来做这道题，利用了位运算，无论是在代码量还是在可读性方面都有了很大的提升。事实上我们没有必要在二进制的形式上进行操作，我们使用移位运算就可以做到按位取反。

------

## Solution

&emsp;&emsp;用`N`直接对2取余其实就是对最后一位取余，用一个`bit`变量记录下从低位开始了多少位，每次取余后进行取反，然后使用`|=`运算结果添加到最后的结果中，然后`N`除以2，在下一个循环可以继续对下一位操作了。

------

## Code

```c++
class Solution {
public:
    int bitwiseComplement(int N) {
        if (N == 0) {
            return 1;
        }
        int result = 0, bit = 0;
        while (N > 0) {
            result |= (N % 2 == 0? 1: 0) << bit++;
            N >>= 1;
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;十进制和二进制的相互转化相信大家都非常熟悉，但是有时候为了提高coding的效率，不一定都要转换才能操作，完全可以用十进制的形式，搭配位运算来达到目的，可能一开始比较难理解，但是用多了就会熟悉，效率提高不少。这道题的分析到这里，谢谢！
