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

&emsp;&emsp;题目是俄罗斯套娃，要求长和宽都必须严格小于才能套进去。很自然的做法是先排序，按照长度从小到大排序，如果长相等，再按照宽来排序。因为我们要找到的套得最多的一种组合，这就有点像子序列问题的，所以用贪心算法是肯定不行的，考虑的方向往dp上靠。

&emsp;&emsp;定义`dp[i]`为选择了第`i`个娃娃所能构成的最大的数量。我们从小到大来遍历，对于每个位置`i`，我们向前遍历`[0, i-1]`，只要满足套娃的要求，就更新状态方程，找到最大的数量。

------

## Solution

&emsp;&emsp;注意初始化dp数组的时候初始值是1，因为单独一个套娃的长度是1，不是0。

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

&emsp;&emsp;这是一道比较简单的dp题目，其本质和字符串子序列题目一样。这道题目的分享到这里，感谢你的支持！
