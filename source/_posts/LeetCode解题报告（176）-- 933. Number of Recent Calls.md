---
title: LeetCode解题报告（176）-- 933. Number of Recent Calls
tags:
  - LeetCode
mathjax: true
date: 2020-10-01 17:14:49


---

## Problem

You have a `RecentCounter` class which counts the number of recent requests within a certain time frame.

Implement the `RecentCounter` class:

- `RecentCounter()` Initializes the counter with zero recent requests.
- `int ping(int t)` Adds a new request at time `t`, where `t` represents some time in milliseconds, and returns the number of requests that has happened in the past `3000` milliseconds (including the new request). Specifically, return the number of requests that have happened in the inclusive range `[t - 3000, t]`.

It is **guaranteed** that every call to `ping` uses a strictly larger value of `t` than the previous call.

<!-- more -->

**Example 1:**

```
Input
["RecentCounter", "ping", "ping", "ping", "ping"]
[[], [1], [100], [3001], [3002]]
Output
[null, 1, 2, 3, 3]

Explanation
RecentCounter recentCounter = new RecentCounter();
recentCounter.ping(1);     // requests = [1], range is [-2999,1], return 1
recentCounter.ping(100);   // requests = [1, 100], range is [-2900,100], return 2
recentCounter.ping(3001);  // requests = [1, 100, 3001], range is [1,3001], return 3
recentCounter.ping(3002);  // requests = [1, 100, 3001, 3002], range is [2,3002], return 3
```

**Constraints:**

- `1 <= t <= 104`
- Each test case will call `ping` with **strictly increasing** values of `t`.
- At most `104` calls will be made to `ping`.

------

## Analysis

&emsp;&emsp;这是一道类的实现题目，意思并不复杂，题目调用`ping(t)`来表示一次call，然后需要返回在这个call之前3000ms内的call的次数，范围是`[t - 3000, t]`。因为题目保证了每次ping的时间是严格递增的，所以我们不需要每次都往前遍历去找出符合这个区间的数字个数，我们只需要找到边界就可以了。

&emsp;&emsp;因为我们需要计算个数，而且计算区间的时候需要用到时间，所以不能单纯地只用一个变量去计数，而是要把数据一个一个存下来。但是注意到，并不是所有的数据都是要一直存储的，比如说当前是t，那么t-3000之前的数据是后面再也用不到的，就不需要存了。

------

## Solution

&emsp;&emsp;配合上面分析的存储特点，这道题目选用队列是最合适的，每次把队头拿出来做比较，如果在区间范围内，则说明后面的都在范围内，这个时候队列的size就是答案了。

&emsp;&emsp;有个坑需要注意，在获取`front()`的时候要先判断是否为空，否则会空指针错误。

------

## Code

```c++
class RecentCounter {
public:
    RecentCounter() {
    
    }
    
    int ping(int t) {
        q.push(t);
        int mark = t - 3000;
        while (q.front() < mark) {
            q.pop();
        }
        return q.size();
    }
private:
    queue<int> q;
};

/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter* obj = new RecentCounter();
 * int param_1 = obj->ping(t);
 */
```

------

## Summary

&emsp;&emsp;这道题目难度不大，使用数组存放，然后每次扫一遍应该也能pass。但是经过仔细的分析，从数据的特点出发去选择最合适的数据结构帮助我们解题，这个才是刷题的意义。这道题的分析到这里，谢谢！
