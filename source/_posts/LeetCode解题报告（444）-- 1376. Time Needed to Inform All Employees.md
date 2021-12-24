---
title: LeetCode解题报告（444）-- 1376. Time Needed to Inform All Employees
mathjax: true
date: 2021-12-23 22:25:18
---

## Problem

A company has `n` employees with a unique ID for each employee from `0` to `n - 1`. The head of the company is the one with `headID`.

Each employee has one direct manager given in the `manager` array where `manager[i]` is the direct manager of the `i-th` employee, `manager[headID] = -1`. Also, it is guaranteed that the subordination relationships have a tree structure.

The head of the company wants to inform all the company employees of an urgent piece of news. He will inform his direct subordinates, and they will inform their subordinates, and so on until all employees know about the urgent news.

The `i-th` employee needs `informTime[i]` minutes to inform all of his direct subordinates (i.e., After informTime[i] minutes, all his direct subordinates can start spreading the news).

Return *the number of minutes* needed to inform all the employees about the urgent news.

<!-- more -->

**Example 1:**

```
Input: n = 1, headID = 0, manager = [-1], informTime = [0]
Output: 0
Explanation: The head of the company is the only employee in the company.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/02/27/graph.png)

```
Input: n = 6, headID = 2, manager = [2,2,-1,2,2,2], informTime = [0,0,1,0,0,0]
Output: 1
Explanation: The head of the company with id = 2 is the direct manager of all the employees in the company and needs 1 minute to inform them all.
The tree structure of the employees in the company is shown.
```

**Constraints:**

- `1 <= n <= 105`
- `0 <= headID < n`
- `manager.length == n`
- `0 <= manager[i] < n`
- `manager[headID] == -1`
- `informTime.length == n`
- `0 <= informTime[i] <= 1000`
- `informTime[i] == 0` if employee `i` has no subordinates.
- It is **guaranteed** that all the employees can be informed.

------

## Analysis

&emsp;&emsp;题目给出一个图结构，给出起始节点，然后每个节点向它的下属传播消息都有一个`informTime`，问需要多久消息才能传播给所有人。这道题目很明显就是一个图的BFS，但是难度在于有一个传播延时。我们来看一个最简单的例子，`1 -> 2 -> 3`，`1 -> 2`的延时是5，`2 -> 3`的延时是2，所以最后3收到的时间应该是7，实际上就是前面的延时的累加。因为我们要保证全局所有人都要收到，所以要维护一个最大的延时，而这个步骤只需要在出度为0的节点做。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int numOfMinutes(int n, int headID, vector<int>& manager, vector<int>& informTime) {
        vector<vector<int>> graph(n);
        for (int i = 0; i < n; i++) {
            if (i == headID) {
                continue;
            }
            graph[manager[i]].push_back(i);
        }
        
        
        int result = 0;
        queue<pair<int, int>> q;
        q.push({0, headID});
        
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                auto cur = q.front();
                q.pop();
                int culTime = cur.first;
                int id = cur.second;
                if (graph[id].size()) {
                    for (int sub: graph[id]) {
                        q.push({culTime + informTime[id], sub});
                    }    
                } else {
                    result = max(culTime, result);
                }
                
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是一道稍微加了一点难度的BFS，整体上还是BFS的框架，里面添加了一个延时。这道题目的分享到这里，感谢你的支持！
