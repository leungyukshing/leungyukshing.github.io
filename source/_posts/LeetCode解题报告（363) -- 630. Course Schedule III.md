---
title: LeetCode解题报告（363) -- 630. Course Schedule III
tags:
  - LeetCode
mathjax: true
date: 2021-05-03 00:31:40

---

## Problem

There are `n` different online courses numbered from `1` to `n`. You are given an array `courses` where `courses[i] = [durationi, lastDayi]` indicate that the `ith` course should be taken **continuously** for `durationi` days and must be finished before or on `lastDayi`.

You will start on the `1st` day and you cannot take two or more courses simultaneously.

Return *the maximum number of courses that you can take*.

<!-- more -->

**Example 1:**

```
Input: courses = [[100,200],[200,1300],[1000,1250],[2000,3200]]
Output: 3
Explanation: 
There are totally 4 courses, but you can take 3 courses at most:
First, take the 1st course, it costs 100 days so you will finish it on the 100th day, and ready to take the next course on the 101st day.
Second, take the 3rd course, it costs 1000 days so you will finish it on the 1100th day, and ready to take the next course on the 1101st day. 
Third, take the 2nd course, it costs 200 days so you will finish it on the 1300th day. 
The 4th course cannot be taken now, since you will finish it on the 3300th day, which exceeds the closed date.
```

**Example 2:**

```
Input: courses = [[1,2]]
Output: 1
```

**Example 3:**

```
Input: courses = [[3,2],[4,3]]
Output: 0
```

**Constraints:**

- `1 <= courses.length <= 104`
- `1 <= durationi, lastDayi <= 104`

------

## Analysis

&emsp;&emsp;这是一道课程编排的系列题，题目给出每门课程的消耗时间及截止时间，要求我们计算出最多能上完几门课。直观上看，我们希望选的课程都是消耗的时间短，并且截止时间比较靠后，所以这道题目是可以使用贪心算法的。

&emsp;&emsp; 首先我们把课程按照截止时间从大到小排序，因为截止时间越大，说明中间可以上的课就越多，所以我们从头开始拿。同时我们维护一个当前课程所需要的总耗时，这个值用来判断是否超过了截止时间。我们每选上一个课程，就需要把它的耗时加到这个总耗时里面。然后做如下的分类处理：

- 当总耗时小于等于当前课程的截止时间，说明是满足要求的，不需要特别处理，直接到下一个；
- 当总耗时大于当前课程的截止时间，说明当前课程上不完了，此时我们需要从**已选的课程中，挑选一个耗时最大的课程替换掉。**这里面的思路是替换掉耗时最大的课程，能有更多的预留空间给后面的课程。

 &emsp;&emsp;根据上面的思路，可以知道这道题目的重点是需要维护一个已选课程的列表，而且能够方便地找到耗时最大的课程。这里很明显就是选用堆了，替换掉耗时最大的课程只需要把堆顶的元素移除即可（记得在总耗时中减去对应的值）。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int scheduleCourse(vector<vector<int>>& courses) {
        sort(courses.begin(), courses.end(), [](vector<int> &a, vector<int> &b) {
            return a[1] < b[1];
        });
        int currentTotalTime = 0;
        priority_queue<int> q;
        for (auto &course: courses) {
            currentTotalTime += course[0];
            q.push(course[0]);
            if (currentTotalTime > course[1]) {
                currentTotalTime -= q.top();
                q.pop();
            }
        }
        return q.size();
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是为数不多直接使用贪心算法求解的题目，涉及到了自定义排序以及堆这两个知识点，难度适中。这道题目的分享到这里，感谢你的支持！
