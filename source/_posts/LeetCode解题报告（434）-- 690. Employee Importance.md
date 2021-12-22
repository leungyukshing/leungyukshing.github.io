---
title: LeetCode解题报告（434）-- 690. Employee Importance
mathjax: true
date: 2021-12-22 16:27:38
---

## Problem

You have a data structure of employee information, including the employee's unique ID, importance value, and direct subordinates' IDs.

You are given an array of employees `employees` where:

- `employees[i].id` is the ID of the `ith` employee.
- `employees[i].importance` is the importance value of the `ith` employee.
- `employees[i].subordinates` is a list of the IDs of the direct subordinates of the `ith` employee.

Given an integer `id` that represents an employee's ID, return *the **total** importance value of this employee and all their direct and indirect subordinates*.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/05/31/emp1-tree.jpg)

```
Input: employees = [[1,5,[2,3]],[2,3,[]],[3,3,[]]], id = 1
Output: 11
Explanation: Employee 1 has an importance value of 5 and has two direct subordinates: employee 2 and employee 3.
They both have an importance value of 3.
Thus, the total importance value of employee 1 is 5 + 3 + 3 = 11.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2021/05/31/emp2-tree.jpg)

```
Input: employees = [[1,2,[5]],[5,-3,[]]], id = 5
Output: -3
Explanation: Employee 5 has an importance value of -3 and has no direct subordinates.
Thus, the total importance value of employee 5 is -3.
```

**Constraints:**

- `1 <= employees.length <= 2000`
- `1 <= employees[i].id <= 2000`
- All `employees[i].id` are **unique**.
- `-100 <= employees[i].importance <= 100`
- One employee has at most one direct leader and may have several subordinates.
- The IDs in `employees[i].subordinates` are valid IDs.

------

## Analysis

&emsp;&emsp;题目给出了一个Employee的数据结构，里面包括`id`、`importance`和`subordinates`，同时给出了一个`id`，要求计算这个职员以及他所有下属的importance之和。这很明显就是一道图相关的问题，因为题目给出了起点，我们只需要做一个遍历即可，这里我采用了BFS。为了方便通过id找到某个职员，我用了一个`unordered_map`去存储`id`和职员的关系。在BFS中把遍历到的所有importance相加即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
/*
// Definition for Employee.
class Employee {
public:
    int id;
    int importance;
    vector<int> subordinates;
};
*/

class Solution {
public:
    int getImportance(vector<Employee*> employees, int id) {
        unordered_map<int, Employee*> graph;
        for (auto &a: employees) {
            graph[a->id] = a;
        }
        unordered_set<int> visited;
        int result = 0;
        queue<int> q;
        q.push(id);
        visited.insert(id);
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                int cur = q.front();
                q.pop();
                //cout << "handle id " << cur << endl;
                result += graph[cur]->importance;
                for (int &next: graph[cur]->subordinates) {
                    if (visited.count(next)) {
                        continue;
                    }
                    visited.insert(next);
                    q.push(next);
                }
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目可以说是一道很典型的BFS，没有额外的内容，非常简单，应该是30s默写。这道题目的分享到这里，感谢你的支持！
