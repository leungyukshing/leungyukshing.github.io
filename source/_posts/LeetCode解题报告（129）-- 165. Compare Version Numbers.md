---
title: LeetCode解题报告（129）-- 165. Compare Version Numbers
tags:
  - LeetCode
mathjax: true
date: 2020-09-10 00:38:11

---

## Problem

Compare two version numbers *version1* and *version2*.
If `version1 > version2` return `1;` if `version1 < version2` return `-1;`otherwise return `0`.

You may assume that the version strings are non-empty and contain only digits and the `.` character.

The `.` character does not represent a decimal point and is used to separate number sequences.

For instance, `2.5` is not "two and a half" or "half way to version three", it is the fifth second-level revision of the second first-level revision.

You may assume the default revision number for each level of a version number to be `0`. For example, version number `3.4` has a revision number of `3` and `4` for its first and second level revision number. Its third and fourth level revision number are both `0`.

<!-- more -->

**Example 1:**

```
Input: version1 = "0.1", version2 = "1.1"
Output: -1
```

**Example 2:**

```
Input: version1 = "1.0.1", version2 = "1"
Output: 1
```

**Example 3:**

```
Input: version1 = "7.5.2.4", version2 = "7.5.3"
Output: -1
```

**Example 4:**

```
Input: version1 = "1.01", version2 = "1.001"
Output: 0
Explanation: Ignoring leading zeroes, both “01” and “001" represent the same number “1”
```

**Example 5:**

```
Input: version1 = "1.0", version2 = "1.0.0"
Output: 0
Explanation: The first version number does not have a third level revision number, which means its third level revision number is default to "0"
```

**Note:**

1. Version strings are composed of numeric strings separated by dots `.` and this numeric strings **may** have leading zeroes.
2. Version strings do not start or end with dots, and they will not be two consecutive dots.

------

## Analysis

&emsp;&emsp;这道题目的本质是字符串的比对，关键的思路就是要分块比对。所以最简单的做法就是先按照`.`切分，然后每一块做对比。注意，这个对比是数字层面的对比，而不是字符串层面的对比，这是因为要去除掉多余的0。

------

## Solution

&emsp;&emsp;C++做字符串的切分没有python等语言这么方便，所以这里使用了`stringstream`。为了成块读取，这里有一个trick，就是在题目给出的字符串最后多加一个`.`，这样就能够按部分来比对了。同时`stringstream`能够直接完成由字符串转换为数字的任务，这也省去了我们再调用`atoi`等函数来做转换了。

------

## Code

```c++
class Solution {
public:
    int compareVersion(string version1, string version2) {
        stringstream s1(version1 + "."), s2(version2 + ".");
        int num1 = 0, num2 = 0;
        
        char dot = '.';
        while (s1.good() || s2.good()) {
            if (s1.good()) {
                s1 >> num1 >> dot;
            }
            if (s2.good()) {
                s2 >> num2 >> dot;
            }
            if (num1 > num2) {
                return 1;
            } else if (num1 < num2) {
                return -1;
            }
            num1 = num2 = 0;
        }
        return 0;
    }
};
```

------

## Summary

&emsp;&emsp;实际上思路并不是很难的题目，用python或者go可以说是秒刷，但是用C++的话还是需要借助一些工具。这道题也给我们提供了C++切分字符串的思路，就是用流。这道题的分析到这里，谢谢！
