---
title: LeetCode解题报告（九）-- 56. Merge Intervals
tags:
  - LeetCode
abbrlink: 23945
date: 2018-09-19 08:28:20
---
## Problem
Given a collection of intervals, merge all overlapping intervals.

<!-- more -->

**Example 1:**

```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

**Example 2:**
```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considerred overlapping.
```
---

## Analysis

### 2018-09-19

&emsp;&emsp;这是一题关于区间操作的问题，与之前[Calendar系列的题目](https://leungyukshing.github.io/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%88%E5%85%AD%EF%BC%89--%20732.%20My%20Calendar%20III.html#more)很类似，因此思路是相似的。通过一个map记录每个关键点（包括区间的起始点和结束点）的**打卡次数**，去识别出重叠的区间，并将他们合并成一个区间。然后将所有的区间组成一个vector返回即可。

### 2020-11-18

&emsp;&emsp;时隔两年之后又碰上了这道题，无论是从思考还是在做法上都有了很大的不同，这里再次做一次题解，刚好可以和之前的形成对比，看到技术成长的过程。

&emsp;&emsp;这道题目要求的是合并重叠的区间，返回的结果是一系列不重叠的区间。首先我们对区间进行排序，排序的标准是先按起点排序，起点相同的情况下再按终点排序，这样就能够按照顺序处理区间。

&emsp;&emsp;使用一个新的数组记录答案，每处理一个区间，和当前答案中最后一个区间做比较。首先判断是否重叠（在有序的情况下，判断重叠的方法是对比前一个区间的终点和后一个区间的起点），如果不重叠直接把当前区间放入答案中；如果重叠的话，就需要计算出两个区间的**并集**，取两个区间终点中最大的作为新区间的终点（起点不需要变化）。

---

## Solution
&emsp;&emsp;同样地，使用一个`cursor`来遍历map。在`cursor`为0的时候，我们认为它正处于一个区间的起始或结束，因此需要在每一个循环前先判断上一个`cursor`是否为0，若是，则从当前点开始是新的区间，用`start`记录开始位置。在`cursor`加上本节点的值后，如果为0，则认为当前处于区间结束点，用`end`记录结束位置。这样我们就能够获得一个完整的区间`[start, end]`。

---

## Code

### 2018-09-19

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
    vector<Interval> merge(vector<Interval>& intervals) {
        map<int, int> records;
        vector<Interval> result;

        for (int i = 0; i < intervals.size(); i++) {
            records[intervals[i].start]++;
            records[intervals[i].end]--;
        }

        map<int, int>::iterator iter;
        int cursor = 0;
        int start = 0, end;
        for(iter = records.begin(); iter != records.end(); iter++) {
            if (cursor == 0) {
                start = iter->first;
            }
            cursor += iter->second;
            if (cursor == 0) {
                end = iter->first;
                Interval tmp = Interval(start, end);
                result.emplace_back(tmp);
            }
        }
        return result;
    }
};
```
**运行时间：**约8ms，超过98.82%的CPP代码

### 2020-11-18

```c++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        int size = intervals.size();
        if (size == 0) {
            return {};
        }
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> result;
        result.push_back(intervals[0]);
        for (int i = 1; i < size; i++) {
            if (result.back()[1] >= intervals[i][0]) {
                result.back()[1] = max(result.back()[1], intervals[i][1]);
            } else {
                result.push_back(intervals[i]);
            }
        }
        return result;
    }
};
```

---

## Summary

&emsp;&emsp;这题已经是我做过的关于区间操作的第4道题目了，可以说在这个方面有了一定的了解。我们可以抛弃以往使用多层嵌套循环的方式来遍历判断区间，因为那样效率很低。使用map的方式反而给我们提供了一种快速的方式，其复杂度也不过是**O(n)**，同时他所能解决的问题范围也大很多，包括区间重叠、区间合并问题等。本题的分析到这里为止，谢谢您的支持！
