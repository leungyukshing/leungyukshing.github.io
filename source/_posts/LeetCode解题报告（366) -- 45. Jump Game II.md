---
title: LeetCode解题报告（366) -- 45. Jump Game II
tags:
  - LeetCode
mathjax: true
abbrlink: 44365
date: 2021-05-10 13:19:22
---

## Problem

Given an array of non-negative integers `nums`, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

You can assume that you can always reach the last index.

<!-- more -->

**Example 1:**

```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: nums = [2,3,0,1,4]
Output: 2
```

**Constraints:**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 105`

------

## Analysis

&emsp;&emsp;题目给出一个数组，要求从第一个位置跳到最后一个位置。在每个位置上，能跳动的最大距离是这个位置上的值。要求我们找出完成的最小的跳数。

&emsp;&emsp;最暴力的解法是BFS，从第一个位置开始，把它能走的所有距离`[1, nums[i]]`都递归计算一遍，最后看看哪个先到达最后一个位置，层数就是答案。但这里有一个很致命的问题，如果数组中的值非常大，那么遍历的数量是很庞大的，没有办法通过oj的检测。

&emsp;&emsp;既然这样，我们就要另想办法了。题目要求的只是返回最少跳了多少次，而对于跳了哪个位置并不care，所以我们并不需要真正地找出那条路径。相反，我们只需要它在数组中到了哪个位置即可。我们维护一个`cur`，它表示的是走到当前位置为止，能跳到的最大的位置。如果一旦`cur >= n - 1`，也就说明能跳到最后一个位置了，这就是终止条件。然后我们用`i`表示当前遍历到了哪个位置。以Example1为例，当`i = 0`时，`cur = 2`，这就是说当遍历完第一个位置后，能跳到的最大位置是2；当`i = 1`时，`cur = 4`，这个也是最后一个位置了；当`i = 2`时，`cur = 3`，比起上一个`cur`，它并不是最大的，所以可以忽略。因此我们每次都通过**上一次计算出来能移动的最大距离去更新i，每次更新完i之后检查cur是否满足终止条件**。因为题目保证了一定能到达最后一个位置，所以不需要考虑边界情况。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        int res = 0, n = nums.size(), i = 0, cur = 0;
        while (cur < n - 1) {
            ++res;
            int pre = cur;
            for (; i <= pre; ++i) {
                cur = max(cur, i + nums[i]);
            }
            if (pre == cur) return -1; // May not need this
        }
        return res;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是有点贪心算法的味道，本质的思路是选择能够提供更远距离的位置。这道题目的分享到这里，感谢你的支持！
