---
title: LeetCode解题报告（179）-- 1288. Remove Covered Intervals
tags:
  - LeetCode
mathjax: true
date: 2020-10-04 22:48:30

---

## Problem

Given a list of `intervals`, remove all intervals that are covered by another interval in the list.

Interval `[a,b)` is covered by interval `[c,d)` if and only if `c <= a` and `b <= d`.

After doing so, return *the number of remaining intervals*.

<!-- more -->

**Example 1:**

```
Input: intervals = [[1,4],[3,6],[2,8]]
Output: 2
Explanation: Interval [3,6] is covered by [2,8], therefore it is removed.
```

**Example 2:**

```
Input: intervals = [[1,4],[2,3]]
Output: 1
```

**Example 3:**

```
Input: intervals = [[0,10],[5,12]]
Output: 2
```

**Example 4:**

```
Input: intervals = [[3,10],[4,10],[5,11]]
Output: 2
```

**Example 5:**

```
Input: intervals = [[1,2],[1,4],[3,4]]
Output: 1
```

**Constraints:**

- `1 <= intervals.length <= 1000`
- `intervals[i].length == 2`
- `0 <= intervals[i][0] < intervals[i][1] <= 10^5`
- All the intervals are **unique**.

------

## Analysis

&emsp;&emsp;这道题目是区间相关的题目，之前在分析类似题目的时候也提到过，解决区间题目最重要的就是抓住区间的两个顶点。这道题目因为要判断的是包含关系，所以我们首先按照起始的点排序，当起始相同的时候，再按照结束的点排序。这样排序过后，如果存在包含关系，那么**被包含的区间一定会在包含的区间之后**。

------

## Solution

&emsp;&emsp;自定义排序条件，按照上面分析的，先排起始，后排结束。然后从头开始遍历，记录下前一个区间的结束坐标，然后和当前的结束坐标做对比。找到一个就把区间数量减少一个，最后得到的就是答案了。

------

## Code

```c++
class Solution {
public:
    int removeCoveredIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [](const vector<int>& x, const vector<int>& y) {
                return x[0] < y[0] || (x[0] == y[0] && x[1] > y[1]);
        });
        int r = -1; // prev end
        int size = intervals.size();
        int result = size;
        for (int i = 0; i < size; i++) {
            if (intervals[i][1] <= r) {
                result--;
            } else {
                r = intervals[i][1];
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;区间类型的题目关键还是把握好区间的头尾，然后善用排序去帮助我们简化判断逻辑。这道题的分析到这里，谢谢！
