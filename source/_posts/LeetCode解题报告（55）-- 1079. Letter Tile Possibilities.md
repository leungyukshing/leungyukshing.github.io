---
title: LeetCode解题报告（55）-- 1079. Letter Tile Possibilities
tags:
  - LeetCode
abbrlink: 58223
date: 2019-09-19 15:20:09
---

## Problem

You have a set of `tiles`, where each tile has one letter `tiles[i]` printed on it.  Return the number of possible non-empty sequences of letters you can make.

<!-- more -->

**Example 1:**

```
Input: "AAB"
Output: 8
Explanation: The possible sequences are "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA".
```

**Example 2:**

```
Input: "AAABBC"
Output: 188
```

**Note:**

1. `1 <= tiles.length <= 7`
2. `tiles` consists of uppercase English letters.

------

## Analysis

&emsp;&emsp;这道题是和排列组合有关的题目，但是由于使用的字符的数量不定，因此不能够用简单的一条公式来推出结果。我们可以从题目给出的例子入手，其中explanation的顺序是从一个字母开始，然后再到两个字母，再到三个，这给我们提供了一个思路：从长度为1开始，通过递归解决问题。

&emsp;&emsp;接下来我们可以看看递归的具体要怎么做。对于相同的字母，我们只关注字母的个数，而不care它们的顺序，所以我们可以通过一个map来维护这个记录。每一个迭代我们从`A`开始，顺序使用每一个**可用字母**。

------

## Solution

&emsp;&emsp; 使用一个长度为26的数组作为一个map，记录每个字母的可用次数。每次迭代的时候使用一个字母，将其可用次数-1，用完后记得加回去（因为可能会有其他的排列组合需要用到）。

------

## Code

```c++
class Solution {
public:
    int numTilePossibilities(string tiles) {
        int* count = new int[26];
        int size = tiles.size();
        
        for (int i = 0; i < 26; i++) {
            count[i] = 0;
        }
        for (int i = 0; i < size; i++) {
            count[tiles[i] - 'A']++;
        }
        return search(count);
    }
    
    int search(int* count) {
        int sum = 0;
        for (int i = 0; i < 26; i++) {
            if (count[i] != 0) {
                sum++;
                count[i]--;
                sum += search(count);
                count[i]++;
            }
        }
        return sum;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道有关排列组合的题目，但是并不是典型的组合题目。从题目的特征入手，可以考虑递归的做法。这道题目的分享到这里，欢迎大家评论、转发，谢谢您的支持！
