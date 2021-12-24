---
title: LeetCode解题报告（331) -- 354. Russian Doll Envelopes
tags:
  - LeetCode
mathjax: true
abbrlink: 36343
date: 2021-04-04 15:52:46
---

## Problem

You are given a 2D array of integers `envelopes` where `envelopes[i] = [wi, hi]` represents the width and the height of an envelope.

One envelope can fit into another if and only if both the width and height of one envelope are greater than the other envelope's width and height.

Return *the maximum number of envelopes you can Russian doll (i.e., put one inside the other)*.

**Note:** You cannot rotate an envelope.

<!-- more -->

**Example 1:**

```
Input: envelopes = [[5,4],[6,4],[6,7],[2,3]]
Output: 3
Explanation: The maximum number of envelopes you can Russian doll is 3 ([2,3] => [5,4] => [6,7]).
```

**Example 2:**

```
Input: envelopes = [[1,1],[1,1],[1,1]]
Output: 1
```

**Constraints:**

- `1 <= envelopes.length <= 5000`
- `envelopes[i].length == 2`
- `1 <= wi, hi <= 104`

------

## Analysis

&emsp;&emsp;题目是俄罗斯套娃，要求长和宽都必须严格小于才能套进去。首先我们先排序，规则是先按长从小到大排序，**当长相等的时候，宽按照降序排序**。后面这个非常重要，因为如果信封的长一样的话并不能相互嵌套，所以宽按降序排序也就避免了这种情况。

&emsp;&emsp;排序完成后，长是有顺序的，问题就可以只focus在宽了，要找一个能包含的，**实际上不就是在找一个最长上升子序列吗？**话不多说，直接把宽这一列拿出来作为一个数组，用二分查找的方法计算一下长度。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int maxEnvelopes(vector<vector<int>>& envelopes) {
        sort(envelopes.begin(), envelopes.end(), [](vector<int> &A, vector<int> &B) {
            if (A[0] < B[0]) {
                return true;
            } else if (A[0] == B[0]) {
                return A[1] < B[1];
            }
            return false;
        });
        int size = envelopes.size();
        vector<int> dp(size, 1);
        
        int result = 0;
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < i; j++) {
                if (envelopes[i][0] > envelopes[j][0] && envelopes[i][1] > envelopes[j][1]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
            result = max(result, dp[i]);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道hard题啊，怎么好像这么快就解决了，这就是套路。排序后我们就把二维的问题转化为一维的问题，然后套用最长上升子序列的解决，完美解决。这道题目的分享到这里，感谢你的支持！
