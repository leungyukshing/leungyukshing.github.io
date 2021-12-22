---
title: LeetCode解题报告（437）-- 846. Hand of Straights
mathjax: true
date: 2021-12-22 17:38:51
---

## Problem

Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of size `groupSize`, and consists of `groupSize` consecutive cards.

Given an integer array `hand` where `hand[i]` is the value written on the `ith` card and an integer `groupSize`, return `true` if she can rearrange the cards, or `false` otherwise.

<!-- more -->

**Example 1:**

```
Input: hand = [1,2,3,6,2,3,4,7,8], groupSize = 3
Output: true
Explanation: Alice's hand can be rearranged as [1,2,3],[2,3,4],[6,7,8]
```

**Example 2:**

```
Input: hand = [1,2,3,4,5], groupSize = 4
Output: false
Explanation: Alice's hand can not be rearranged into groups of 4.
```

**Constraints:**

- `1 <= hand.length <= 104`
- `0 <= hand[i] <= 109`
- `1 <= groupSize <= hand.length`

------

## Analysis

&emsp;&emsp;题目给出一个数组`hand`，还有一个`groupSize`，要求把数组`hand`中的数字分成若干组，每组有`groupSize`个数字，并且这些数字都是连续的。因为考虑到是连续的，所以很自然会想起排序。先看看如果是在纸上写会怎么解这道题目，我会把数字先排序好，然后从头开始选`groupSize`个连续的，然后再往后走，如果刚刚好数字都用完了就return true，否则return false。因为有些数字可以出现多次，这里我就直接用`map`了，同时兼具了排序和统计的功能。

&emsp;&emsp;然后我们遍历这个map，我们首先确保连续，对于每一个key，往后找`groupSize - 1`个数字（加上key就是groupSize个数字），然后再在map中查看这些数字是否存在。为了解决重复的情况，比如`[1,1,2,2,3,3]`，我们直接在处理`1`的时候就把次数考虑进去，往后找的时候如果数字的频率小于当前的频率，说明不足以凑成连续的子数组，可以直接return false。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    bool isNStraightHand(vector<int>& hand, int groupSize) {
        map<int, int> freq;
        for (int &a: hand) {
            ++freq[a];
        }
        
        for (auto [k, v]: freq) {
            if (v == 0) {
                continue;
            }
            for (int i = k; i < k + groupSize; ++i) {
                if (freq[i] < v) {
                    return false;
                }
                freq[i] -= v;
            }
        }
        return true;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目主要还是考察了对map的使用和理解，没有特别难的地方。这道题目的分享到这里，感谢你的支持！
