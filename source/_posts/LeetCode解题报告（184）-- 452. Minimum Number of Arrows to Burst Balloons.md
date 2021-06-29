---
title: LeetCode解题报告（184）-- 452. Minimum Number of Arrows to Burst Balloons
tags:
  - LeetCode
mathjax: true
abbrlink: 22058
date: 2020-10-10 18:00:20
---

## Problem

There are some spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter, and hence the x-coordinates of start and end of the diameter suffice. The start is always smaller than the end.

An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with `xstart` and `xend` bursts by an arrow shot at `x` if `xstart ≤ x ≤ xend`. There is no limit to the number of arrows that can be shot. An arrow once shot keeps traveling up infinitely.

Given an array `points` where `points[i] = [xstart, xend]`, return *the minimum number of arrows that must be shot to burst all balloons*.

<!-- more -->

**Example 1:**

```
Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).
```

**Example 2:**

```
Input: points = [[1,2],[3,4],[5,6],[7,8]]
Output: 4
```

**Example 3:**

```
Input: points = [[1,2],[2,3],[3,4],[4,5]]
Output: 2
```

**Example 4:**

```
Input: points = [[1,2]]
Output: 1
```

**Example 5:**

```
Input: points = [[2,3],[2,3]]
Output: 1
```

**Constraints:**

- `0 <= points.length <= 104`
- `points.length == 2`
- `-231 <= xstart < xend <= 231 - 1`

------

## Analysis

&emsp;&emsp;这道题目的描述有点复杂，但本质就是区间问题。先简单解释一下题目：在二维空间中有许多气球，给出了每个气球横坐标的起点和终点。然后现在在纵坐标方向发射若干支箭，问要发射多少支才能把气球都戳爆。

&emsp;&emsp;如果气球在横坐标上都没有重叠，那么肯定就是一个气球一支箭，所以我们要解决的问题就是在横坐标上重叠的情况。我们只需要在重叠的区间中发射箭，就能够做到一支箭戳爆多个气球的效果，所以题目就转化为求重叠区间的个数了。

&emsp;&emsp;首先按照起点从小到大对区间进行排序，然后从第一个区间开始一直找。如果区间有重叠的话，就更换成重叠的区间，起点取两个区间较大的，终点取两个区间较小的，最后重叠区间的个数就是需要箭的数量。

------

## Solution

&emsp;&emsp;同样地这里对区间的排序用到了lambda表达式，其余的没有太多的技巧。

------

## Code

```c++
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(), points.end(), [](vector<int> a, vector<int> b) {
            return a[0] < b[0] || (a[0] == b[0] && a[1] < b[1]);
        });
        
        int size = points.size();
        if (size == 0) {
            return 0;
        }
        vector<vector<int>> temp;
        temp.push_back(points[0]);
        int lastEnd = temp[0][1];
        int idx = 0;
        for (int i = 1; i < size; i++) {
            if (points[i][0] <= lastEnd) {
                vector<int> t;
                t.push_back(max(temp[idx][0], points[i][0]));
                t.push_back(min(temp[idx][1], points[i][1]));
                lastEnd = min(temp[idx][1], points[i][1]);
                temp.pop_back();
                temp.push_back(t);
            } else {
                lastEnd = points[i][1];
                temp.push_back(points[i]);
                idx++;
            }
        }
        
        return temp.size();
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难点在于转换题目到我们熟悉的思路上，转换后其实求解问题是比较简单的，就单纯是一个区间重叠问题。这道题目的分享到这里，谢谢！
