---
title: LeetCode解题报告（28）-- 57. Insert Interval
tags:
  - LeetCode
abbrlink: 20039
date: 2018-11-06 09:31:16
---
## Problem
Given a set of non-overlapping intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.
<!-- more -->

**Example 1:**
```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2:**
```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

## Analysis
&emsp;&emsp;这道题要求我们将一个新的区间插入到一系列区间中，其中牵涉到了overlap的问题。我们之前也做过关于区间重叠的问题，这里对区间重叠的情况就不详细展开讨论。如果新的区间和区间序列中的所有区间都不重叠，那么直接插入即可，这种情况是比较简单的。如果新的区间和区间序列中的部分区间重叠，我们需要确定合并区间，这里就稍微复杂了。区间的起点需要取两者小的，区间的终点需要取两者大的，我们不断更新这个新区间的范围，以足够涵盖所有覆盖到的区间。
&emsp;&emsp;根据上面的分析，我们能够得到出最终插入的区间，但是插入的位置呢？如果是第一种情况，我们在`newInterval.start > intervals[i].end`的情况下，需要在插入当前区间后再插入新区间，因此这里就要用到一个额外的变量`ptr`来记录要新区间插入的位置。如果是第二种情况，因为新区间合并了部分区间，直接插入到其合并的第一个区间的位置就可以了。

## Solution
&emsp;&emsp;对于`intervals`进行遍历，分下面三种情况讨论：
  + `newInterval.start > intervals[i].end`，两个区间不重叠，新区间应该在当前区间`intervals[i]`后插入，因此`ptr++`。
  + `newInterval.end < intervals[i].start`，两个区间不重叠，新区间在当前区间`intervals[i]`前插入，因此`ptr`不需要改变。
  + 重叠的情况，需要不断更新`newInterval`的`start`和`end`，因为新区间有可能与多个区间重叠。每次更新遵循以下准则：
    + `newInterval.start = min(intervals[i].start, newInterval.start);`
    + `newInterval.end =  max(intervals[i].end, newInterval.end);`

## Code
```C++
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
class Solution {
public:
    vector<Interval> insert(vector<Interval>& intervals, Interval newInterval) {
        vector<Interval> result;
        int ptr = 0;
        for (int i = 0; i < intervals.size(); i++) {
            // No Overlap
            if (intervals[i].start > newInterval.end) {
                result.emplace_back(intervals[i]);
            }
            else if (intervals[i].end < newInterval.start) {
                ptr++;
                result.emplace_back(intervals[i]);
            }
            else {
                newInterval.start = min(intervals[i].start, newInterval.start);
                newInterval.end =  max(intervals[i].end, newInterval.end);
            }
        }
        result.insert(result.begin() + ptr, newInterval);
        return result;
    }
};
```
**运行时间：**约8ms，超过99.20%的CPP代码。

## Summary
&emsp;&emsp;这道题是涉及到区间重叠问题比较难的一题，原因在于新区间很容易与多个区间重叠。一开始没注意到就做错了，后来再仔细分析，发现插入一个新的区间，最多会在原来的数组中插入一个区间。不重叠是就保留原来的区间，重叠时就放弃原来的区间，更新当前区间。确定好这个思路后编程就很容易了。因此涉及到区间问题，还是要多在纸上用时间轴的形式来演算。本题的分析到这里，谢谢您的支持！
