---
title: LeetCode解题报告（48）-- 933. Number of Recent Calls
tags:
  - LeetCode
abbrlink: 51459
date: 2019-09-14 21:46:32
---

## Problem

Write a class `RecentCounter` to count recent requests.

It has only one method: `ping(int t)`, where t represents some time in milliseconds.

Return the number of `ping`s that have been made from 3000 milliseconds ago until now.

Any ping with time in `[t - 3000, t]` will count, including the current ping.

It is guaranteed that every call to `ping` uses a strictly larger value of `t` than before.

<!-- more -->

**Example 1:**

```
Input: inputs = ["RecentCounter","ping","ping","ping","ping"], inputs = [[],[1],[100],[3001],[3002]]
Output: [null,1,2,3,3]
```

**Note:**

1. Each test case will have at most `10000` calls to `ping`.
2. Each test case will call `ping` with strictly increasing values of `t`.
3. Each call to ping will have `1 <= t <= 10^9`.

------

## Analysis

&emsp;&emsp;这道题目是一个流程题，输入是一系列的操作，输出的是在最近3000ms里操作的数量。题目的难点在于这个**最近3000ms**的限制，一个最基本的想法就是用一个vector来保存所有的ping记录，然后每次都遍历一遍，把满足这个限制的操作次数统计起来。

&emsp;&emsp;通过分析题目的特点，我们应该可以优化一下上面的做法。因为ping操作是按时间顺序的，具有时序性特点的数据我们可以优先考虑使用**队列**，因为队列本身具有的特性就是按照一定的顺序。然后再进一步分析，地对于所有的ping记录，只有距离当前3000ms以内的数据是对我们有用的，超出这个范围的旧数据在之后是不会被计算到的，所以我们可以`pop()`掉旧的数据，这样我们的数据量也不会一直增大。

------

## Solution

&emsp;&emsp;具体做法是，使用一个queue存放ping的记录的时间，每接受一个ping，首先`push()`到队尾，然后`pop()`掉过期的数据（即距离当前时间大于3000ms的数据），这样维护的队列中存放的就是访问时间是最近3000ms的所有操作，我们直接返回队列的元素个数即可。

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

&emsp;&emsp;这是一道流程题，一般来说根据流程一步一步走就能够解决。这道题的价值在于我们在解决与时间顺序有关系的问题时，可以优先考虑使用队列，这样可以减少数据占用的空间和时间。希望这道题能够帮助到你，欢迎大家分享、评论，谢谢！
