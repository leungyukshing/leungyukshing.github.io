---
title: LeetCode解题报告（六）-- 732. My Calendar III
tags:
  - LeetCode
abbrlink: 37394
date: 2018-09-11 16:18:37
---
## Problem
Implement a `MyCalendarThree` class to store your events. A new event can always be added.

Your class will have one method, `book(int start, int end)`. Formally, this represents a booking on the half open interval `[start, end)`, the range of real numbers `x` such that `start <= x < end`.

A K-booking happens when **K** events have some non-empty intersection (ie., there is some time that is common to all K events.)

For each call to the method `MyCalendar.book`, return an integer `K` representing the largest integer such that there exists a `K`-booking in the calendar.

Your class will be called like this: `MyCalendarThree cal = new MyCalendarThree()`; `MyCalendarThree.book(start, end)`
<!-- more -->

**Example1:**
```
MyCalendarThree();
MyCalendarThree.book(10, 20); // returns 1
MyCalendarThree.book(50, 60); // returns 1
MyCalendarThree.book(10, 40); // returns 2
MyCalendarThree.book(5, 15); // returns 3
MyCalendarThree.book(5, 10); // returns 3
MyCalendarThree.book(25, 55); // returns 3
Explanation:
The first two events can be booked and are disjoint, so the maximum K-booking is a 1-booking.
The third event [10, 40) intersects the first event, and the maximum K-booking is a 2-booking.
The remaining events cause the maximum K-booking to be only a 3-booking.
Note that the last event locally causes a 2-booking, but the answer is still 3 because
eg. [10, 20), [10, 40), and [5, 15) are still triple booked.
```

**Note:**
  + The number of calls to `MyCalendarThree.book` per test case will be at most `400`.
  + In calls to `MyCalendarThree.book(start, end)`, `start` and `end` are integers in the range `[0, 10^9]`.


## Analysis
&emsp;&emsp;本题是Calendar系列的第三题，难度比起前两题有了提升，但是本质上是一样的。我在[Calendar Ⅱ](https://leungyukshing.github.io/archives/LeetCode%E8%A7%A3%E9%A2%98%E6%8A%A5%E5%91%8A%EF%BC%88%E4%BA%94%EF%BC%89--%20731.%20My%20Calendar%20II.html)中提到了一个多级的集合，这道题的任务就是统计多级集合的级数。但是由于区间重叠的次数是未知的，而且，大量创建vector的会使得代码的效率降低，因此我寻求是否有另一种更加高效简便的方法。
&emsp;&emsp;答案当然是有的。我们先来考虑一个情景：一个人参加多个视频会议，每次进入会议室之前他必须先输入账号和密码（登陆），每次结束会议后需要输入账号和密码（登出），那么在任意时刻中，我们只需要统计当前账户登陆的次数就可以知道，该人同时在参加多少个会议。
&emsp;&emsp;回到这道题，对于整一个时间轴，我们使用一个map来存储，key是事件的`start`或`end`，value是该时间点**在线**的次数。于是，我们就得到了一条时间轴，我们只需要找出在时间轴上最大的**在线**数量，即是答案了。

## Solution
&emsp;&emsp;使用map来存储一条时间轴，原因是map具有对key自动排序的功能。对于每一个需要判断的区间，我们首先在时间轴上进行**登录（+1）**和**登出（-1）**操作。然后我们遍历这个map，使用一个`cursor`加上每个时间点对应的value，找出其中最大的**在线**次数即可。

## Code
```C++
class MyCalendarThree {
public:
    MyCalendarThree() {

    }

    int book(int start, int end) {
        timeline[start]++;
        timeline[end]--;

        int max = 0;
        int cursor = 0;
        for(auto iter = timeline.begin(); iter != timeline.end(); iter++) {
            cursor += iter->second;
            max = max >= cursor ? max : cursor;
        }
        return max;
    }

private:
    map<int, int> timeline;
};

/**
 * Your MyCalendarThree object will be instantiated and called as such:
 * MyCalendarThree obj = new MyCalendarThree();
 * int param_1 = obj.book(start,end);
 */
```
**运行时间：**88ms，超过40.97%的CPP代码。

## Summary
&emsp;&emsp;Calendar系列的三道题是难度递增的，他们的思路也是一致的。难点还是在第三题中，如何把问题抽象出一个开会的模式，只要理解了这个模式，实现起来就方便很多了。本题的分析就到这里，谢谢！
