---
title: LeetCode解题报告（五）-- 731. My Calendar II
tags:
  - LeetCode
abbrlink: 18673
date: 2018-09-10 22:13:27
---
## Problem
Implement a `MyCalendarTwo` class to store your events. A new event can be added if adding the event will not cause a **triple** booking.

Your class will have one method, `book(int start, int end)`. Formally, this represents a booking on the half open interval `[start, end)`, the range of real numbers `x` such that `start <= x < end`.

A triple booking happens when **three** events have some non-empty intersection (ie., there is some time that is common to all 3 events.)

For each call to the method `MyCalendar.book`, return `true` if the event can be added to the calendar successfully without causing a triple booking. Otherwise, return `false` and do not add the event to the calendar.

Your class will be called like this: `MyCalendar cal = new MyCalendar()`; `MyCalendar.book(start, end)`
<!-- more -->

**Example1:**
```
MyCalendar();
MyCalendar.book(10, 20); // returns true
MyCalendar.book(50, 60); // returns true
MyCalendar.book(10, 40); // returns true
MyCalendar.book(5, 15); // returns false
MyCalendar.book(5, 10); // returns true
MyCalendar.book(25, 55); // returns true
Explanation:
The first two events can be booked.  The third event can be double booked.
The fourth event (5, 15) can't be booked, because it would result in a triple booking.
The fifth event (5, 10) can be booked, as it does not use time 10 which is already double booked.
The sixth event (25, 55) can be booked, as the time in [25, 40) will be double booked with the third event;
the time [40, 50) will be single booked, and the time [50, 55) will be double booked with the second event.
```

**Note:**
  + The number of calls to `MyCalendar.book` per test case will be at most `1000`.
  + In calls to `MyCalendar.book(start, end)`, `start` and `end` are integers in the range `[0, 10^9]`.



## Analysis
&emsp;&emsp;这是Calendar系列的第二题，题目要求我们判断会议是否**三重重叠**。也就是说，没有重叠和两个会议重叠都是允许的，三个会议重叠则不允许。如果完成了[Calendar Ⅰ](http://leungyukshing.cn/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%88%E5%9B%9B%EF%BC%89--%20729.%20My%20Calendar%20I.html)，这题会比较简单。判断重叠的条件与之前一样，关键在于需要记录不重叠区间和两个会议重叠的区间。

## Solution
&emsp;&emsp;我们需要两个vector来模拟一个分级的集合，分别记录不重叠的区间和一次重叠的区间。对于每个需要判断的区间，我们首先在一次重叠的vector中进行对比，如果存在交集，则说明出现了三个会议重叠的情况，直接return `false`；否则，从无重叠的区间中对比，如果存在重叠，则将重叠的区间加入到一次重叠的vector中，否则，直接把区间加入到不重叠的vector中。需要注意，**两个区间的重叠区间是左边界取大，右边界取小**！

## Code
```C++
class MyCalendarTwo {
public:
    MyCalendarTwo() {

    }

    bool book(int start, int end) {
        for (int i = 0; i < overlaps.size(); i++) {
            if (!(end <= overlaps[i].first || start >= overlaps[i].second)) {
                return false;
            }
        }
        for (int i = 0; i < records.size(); i++) {
            if (!(end <= records[i].first || start >= records[i].second)) {
                overlaps.emplace_back(make_pair(max(start, records[i].first), min(end, records[i].second)));
            }
        }
        records.emplace_back(make_pair(start, end));
        return true;
    }

private:
    vector<pair<int, int>> overlaps, records;
};

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * MyCalendarTwo obj = new MyCalendarTwo();
 * bool param_1 = obj.book(start,end);
 */
```
**运行时间：**56ms，超过83.27%的CPP代码。

## Summary
&emsp;&emsp;这道题运用了一个分级的思想，将重叠程度不同的区间分集合存储，关键点是提取出重叠的区间加入下一级的vector。分析清楚后，其实coding过程没有什么坑。个人建议大家在动手之前还是在纸上写一下大概的思路，这样会高效很多。这道题的分享就到这里，谢谢！
