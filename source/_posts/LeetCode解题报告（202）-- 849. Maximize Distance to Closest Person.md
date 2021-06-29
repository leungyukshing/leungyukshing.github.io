---
title: LeetCode解题报告（202）-- 849. Maximize Distance to Closest Person
tags:
  - LeetCode
mathjax: true
abbrlink: 32415
date: 2020-10-29 18:20:44
---

## Problem

You are given an array representing a row of `seats` where `seats[i] = 1` represents a person sitting in the `ith` seat, and `seats[i] = 0` represents that the `ith` seat is empty **(0-indexed)**.

There is at least one empty seat, and at least one person sitting.

Alex wants to sit in the seat such that the distance between him and the closest person to him is maximized. 

Return *that maximum distance to the closest person*.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/09/10/distance.jpg)

```
Input: seats = [1,0,0,0,1,0,1]
Output: 2
Explanation: 
If Alex sits in the second open seat (i.e. seats[2]), then the closest person has distance 2.
If Alex sits in any other open seat, the closest person has distance 1.
Thus, the maximum distance to the closest person is 2.
```

**Example 2:**

```
Input: seats = [1,0,0,0]
Output: 3
Explanation: 
If Alex sits in the last seat (i.e. seats[3]), the closest person is 3 seats away.
This is the maximum distance possible, so the answer is 3.
```

**Example 3:**

```
Input: seats = [0,1]
Output: 1
```

**Constraints:**

- `2 <= seats.length <= 2 * 104`
- `seats[i]` is `0` or `1`.
- At least one seat is **empty**.
- At least one seat is **occupied**.

------

## Analysis

&emsp;&emsp;题目要求计算去离最近一个坐着的人最大的距离，首先我们要看一下这个距离到底是怎么计算出来的。如果只有一边有人坐的话，自然是距离这个人越远越好。但是如果两边都有人的话，这时**最大距离是出现在中间的时候**。所以用1把数组分为若干个区间，我们只需要逐个区间去计算一半的区间长度，最后取一个最大值就可以了。

------

## Solution

&emsp;&emsp;用两个下标表示区间，每找到一个区间就进行计算，最后处理一下只有一个1的情况即可。

------

## Code

```c++
class Solution {
public:
    int maxDistToClosest(vector<int>& seats) {
        int left = 0, right = 0, result = 0;
        int size = seats.size();
        
        for (left = 0, right = 0; right < size; right++) {
            if (seats[right] == 1) {
                if (left == 0) {
                    result = max(result, right - left);
                } else {
                    result = max(result, (right - left + 1) / 2);
                }
                left = right + 1;
            }
        }
        return max(result, size - left);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是较为简单的基于数组的区间类题目，关键就是把握好起点和终点，然后是区间的移动，以及边界情况，coding上并无特别难的地方。这道题目的分享到这里，谢谢！
