---
title: LeetCode解题报告（九）-- 56. Merge Intervals
tags:
  - LeetCode
abbrlink: 23945
date: 2018-09-19 08:28:20
---
## Problem
Given a collection of intervals, merge all overlapping intervals.

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
<!-- more -->

## Analysis
&emsp;&emsp;这是一题关于区间操作的问题，与之前[Calendar系列的题目](https://leungyukshing.github.io/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%88%E5%85%AD%EF%BC%89--%20732.%20My%20Calendar%20III.html#more)很类似，因此思路是相似的。通过一个map记录每个关键点（包括区间的起始点和结束点）的**打卡次数**，去识别出重叠的区间，并将他们合并成一个区间。然后将所有的区间组成一个vector返回即可。
## Solution
&emsp;&emsp;同样地，使用一个`cursor`来遍历map。在`cursor`为0的时候，我们认为它正处于一个区间的起始或结束，因此需要在每一个循环前先判断上一个`cursor`是否为0，若是，则从当前点开始是新的区间，用`start`记录开始位置。在`cursor`加上本节点的值后，如果为0，则认为当前处于区间结束点，用`end`记录结束位置。这样我们就能够获得一个完整的区间`[start, end]`。
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

## Summary
&emsp;&emsp;这题已经是我做过的关于区间操作的第4道题目了，可以说在这个方面有了一定的了解。我们可以抛弃以往使用多层嵌套循环的方式来遍历判断区间，因为那样效率很低。使用map的方式反而给我们提供了一种快速的方式，其复杂度也不过是**O(n)**，同时他所能解决的问题范围也大很多，包括区间重叠、区间合并问题等。本题的分析到这里为止，谢谢您的支持！
