---
title: LeetCode解题报告（186）-- 859. Buddy Strings
tags:
  - LeetCode
mathjax: true
abbrlink: 53826
date: 2020-10-12 19:34:40
---

## Problem

Given two strings `A` and `B` of lowercase letters, return `true` *if you can swap two letters in* `A` *so the result is equal to* `B`*, otherwise, return* `false`*.*

Swapping letters is defined as taking two indices `i` and `j` (0-indexed) such that `i != j` and swapping the characters at `A[i]` and `A[j]`. For example, swapping at indices `0` and `2` in `"abcd"` results in `"cbad"`.

<!-- more -->

**Example 1:**

```
Input: A = "ab", B = "ba"
Output: true
Explanation: You can swap A[0] = 'a' and A[1] = 'b' to get "ba", which is equal to B.
```

**Example 2:**

```
Input: A = "ab", B = "ab"
Output: false
Explanation: The only letters you can swap are A[0] = 'a' and A[1] = 'b', which results in "ba" != B.
```

**Example 3:**

```
Input: A = "aa", B = "aa"
Output: true
Explanation: You can swap A[0] = 'a' and A[1] = 'a' to get "aa", which is equal to B.
```

**Example 4:**

```
Input: A = "aaaaaaabc", B = "aaaaaaacb"
Output: true
```

**Example 5:**

```
Input: A = "", B = "aa"
Output: false
```

------

## Analysis

&emsp;&emsp;连续两天都是字符串类型的题目，这道题目要求在一个字符串上做一次交换操作（必须做一次），变换后能否与另一个字符串相同。因为要交换两个位置上的字符，所以我们首先要找到这两个位置。

&emsp;&emsp;先考虑最常见的情况，就是两个位置的值不一样，这样通过逐个比对`A`和`B`两个字符串，就能找到字符不相同的位置，如果不同的位置只有一个或者超过了两个，则说明没有办法通过一次交换使得两个字符串相同，这两种情况直接return false。然后考虑刚好有两个位置不同的情况，这种情况直接看看交换的字符是不是相同的就可以了，如果是相同则return true，否则return false。

&emsp;&emsp;上面的几种情况判断都不难，比较坑的是一些edge case，当两个字符串完全相同的时候，也不一定代表变换后就能相同。比如`aaab`和`aaab`，因为有重复的`a`，所以重复的`a`相互交换并不会影响结果，一次交换后两个字符串仍然是相同的；但是`abc` 和`abc`这种就不可以，因为必须要交换一次，交换后两个字符串就不能相等了。所以在判断的时候还要判断字符串中是否有重复的元素。

------

## Solution

&emsp;&emsp;判断是否有重复的元素的时候用到了map，其余的并没有使用其他数据结构了。

------

## Code

```c++
class Solution {
public:
    bool buddyStrings(string A, string B) {
        
        int sizeA = A.size(), sizeB = B.size();
        if (sizeA != sizeB) {
            return false;
        }
        map<char, int> m;
        for (int i = 0; i < sizeA; i++) {
            m[A[i]]++;
        }
        bool hasSameEle = false;
        for (auto &temp: m) {
            if (temp.second > 1) {
                hasSameEle = true;
            }
        }
        int idx0 = -1;
        int idx1 = -2;
        
        for (int i = 0; i < sizeA; i++) {
            if (A[i] != B[i]) {
                if (idx0 == -1) {
                    idx0 = i;
                } else if (idx1 == -2) {
                    idx1 = i;
                } else {
                    return false;
                }
            }
        }
        
        if (A == B && hasSameEle) {
            return true;
        }
        if (idx0 < 0 || idx1 < 0) {
            return false;
        }
        
        if (A[idx0] == B[idx1] && A[idx1] == B[idx0]) {
            return true;
        } else {
            return false;
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题在coding层面并不难，但是要考虑多种case才能完全pass。这道题米也是告诉我们在刷题的时候不能过度依赖题目给出的test case，自己在写题的时候也应该思考怎么写test case，这样才能更好地提高代码质量。这道题目的分享到这里，谢谢！
