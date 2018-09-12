---
title: LeetCode解题报告（四）-- 729. My Calendar I
tags:
  - LeetCode
abbrlink: 33235
date: 2018-09-10 14:27:34
---
## Problem
Implement a `MyCalendar` class to store your events. A new event can be added if adding the event will not cause a double booking.

Your class will have the method, `book(int start, int end)`. Formally, this represents a booking on the half open interval `[start, end)`, the range of real numbers `x` such that `start <= x < end`.

A double booking happens when two events have some non-empty intersection (ie., there is some time that is common to both events.)

For each call to the method `MyCalendar.book`, return `true` if the event can be added to the calendar successfully without causing a double booking. Otherwise, return `false` and do not add the event to the calendar.

Your class will be called like this: `MyCalendar cal = new MyCalendar();` `MyCalendar.book(start, end)`
<!-- more -->

**Example 1:**
```
MyCalendar();
MyCalendar.book(10, 20); // returns true
MyCalendar.book(15, 25); // returns false
MyCalendar.book(20, 30); // returns true
Explanation:
The first event can be booked.  The second can't because time 15 is already booked by another event.
The third event can be booked, as the first event takes every time less than 20, but not including 20.
```

**Note:**
  + The number of calls to `MyCalendar.book` per test case will be at most `1000`.
  + In calls to `MyCalendar.book(start, end)`, `start` and `end` are integers in the range `[0, 10^9]`.

## Analysis
&emsp;&emsp;My Calendar总共有三道题，考察的重点是对于区间重叠的判断，而难点在于如何使用更少的循环求解出答案。这篇post分析的是第一题，也就是判断区间之间是否重合。我们很自然地会思考如何判断区间是否重叠。这里给出区间重叠的四种情况：
![区间重叠](/images/区间重叠.jpg)

&emsp;&emsp;但其实对于写代码而言，更快的方法应该是判断区间不重叠，然后取非就可以了，这里给出区间不重叠的两种情况。
![区间分离](/images/区间分离.jpg)

## Solution
&emsp;&emsp;根据上面给出的区间重叠的情况，主要抓住`start`和`end`的位置，就可以快速判断出区间之间是否重叠。对于重叠区间，直接返回fasle；对于不重叠区间，我们需要将他加入到现有的vector中。

## Code
```C++
class MyCalendar {
public:
    MyCalendar() {

    }

    bool book(int start, int end) {
        for(int i = 0; i < records.size(); i++) {
            if (!(start >= records[i].second || end <= records[i].first)) {
                return false;
            }
        }
        records.emplace_back(make_pair(start, end));
        return true;
    }
private:
    vector<pair<int, int> > records;
};

/**
 * Your MyCalendar object will be instantiated and called as such:
 * MyCalendar obj = new MyCalendar();
 * bool param_1 = obj.book(start,end);
 */
```
**运行时间：**76ms，超过44.04%的CPP代码。

## Summary
&emsp;&emsp;本题的难度本身不大，但是鉴于这是一个系列题，因此准确地分析出区间重叠的情况对于接下来求解第二题至关重要。这道题的分析就到这里，谢谢！
