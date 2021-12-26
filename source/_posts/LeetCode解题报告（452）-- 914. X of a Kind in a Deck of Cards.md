---
title: LeetCode解题报告（452）-- 914. X of a Kind in a Deck of Cards
mathjax: true
date: 2021-12-25 22:47:05
---

## Problem

In a deck of cards, each card has an integer written on it.

Return `true` if and only if you can choose `X >= 2` such that it is possible to split the entire deck into 1 or more groups of cards, where:

- Each group has exactly `X` cards.
- All the cards in each group have the same integer.

<!-- more -->

**Example 1:**

```
Input: deck = [1,2,3,4,4,3,2,1]
Output: true
Explanation: Possible partition [1,1],[2,2],[3,3],[4,4].
```

**Example 2:**

```
Input: deck = [1,1,1,2,2,2,3,3]
Output: false
Explanation: No possible partition.
```

**Constraints:**

- `1 <= deck.length <= 104`
- `0 <= deck[i] < 104`

## Analysis

&emsp;&emsp;题目给出一个数组`deck`，要求把里面相同的数字分成若干堆，每堆中的数字都是一样的，并且每组有`X`张（$X \ge2$）卡片。这里有一个误区要理清楚，就是一堆中只能有一种卡片，但是一种卡片不一定只在一堆。比如我有4个2，我可以有两堆，每堆2个。对于这种相同数字放一块的题目，通常的做法先用一个map来统计frequency，但是并不是有多少种数字就是多少个堆，而是要**找各种数字数量的最小公倍数**。如果最后找到的最小公倍数大于等于2，说明满足题目，return true，反之return false。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    bool hasGroupsSizeX(vector<int>& deck) {
        unordered_map<int, int> m;
        int minFreq = INT_MAX;
        for (int num: deck) {
            m[num]++;
        }
        
        int g = -1;
        for (auto &[k, v]: m) {
            if (g == -1) {
                g = v;
            } else {
                g = gcd(g, v);
            }
        }
        return g >= 2;
    }
private:
    int gcd(int x, int y) {
        return x == 0 ? y: gcd(y % x, x);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目我一开始做的时候忽略了一种卡片可以放成多个堆，所以一直没做出来，后来发现这个问题后，转化为用最小公倍数计算就很快解决。这道题目的分享到这里，感谢你的支持！
