---
title: LeetCode解题报告（435）-- 1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit
mathjax: true
date: 2021-12-22 16:32:12
---

## Problem

Given an array of integers `nums` and an integer `limit`, return the size of the longest **non-empty** subarray such that the absolute difference between any two elements of this subarray is less than or equal to `limit`*.*

<!-- more -->

**Example 1:**

```
Input: nums = [8,2,4,7], limit = 4
Output: 2 
Explanation: All subarrays are: 
[8] with maximum absolute diff |8-8| = 0 <= 4.
[8,2] with maximum absolute diff |8-2| = 6 > 4. 
[8,2,4] with maximum absolute diff |8-2| = 6 > 4.
[8,2,4,7] with maximum absolute diff |8-2| = 6 > 4.
[2] with maximum absolute diff |2-2| = 0 <= 4.
[2,4] with maximum absolute diff |2-4| = 2 <= 4.
[2,4,7] with maximum absolute diff |2-7| = 5 > 4.
[4] with maximum absolute diff |4-4| = 0 <= 4.
[4,7] with maximum absolute diff |4-7| = 3 <= 4.
[7] with maximum absolute diff |7-7| = 0 <= 4. 
Therefore, the size of the longest subarray is 2.
```

**Example 2:**

```
Input: nums = [10,1,2,4,7,2], limit = 5
Output: 4 
Explanation: The subarray [2,4,7,2] is the longest since the maximum absolute diff is |2-7| = 5 <= 5.
```

**Example 3:**

```
Input: nums = [4,2,2,2,4,4,2,2], limit = 0
Output: 3
```

**Constraints:**

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`
- `0 <= limit <= 109`

------

## Analysis

&emsp;&emsp;题目给出一个数组`nums`以及一个`limit`，要求找出最长的子数组，子数组中任意两个数的绝对值不超过`limit`。首先是数组类型的题目，而且是子数组，要求返回一个长度，这三个信息很明显指向了sliding window。而绝对值的要求则是限制条件。

&emsp;&emsp;先把sliding window的框架套上去，再来看滑动过程中我们需要维护什么信息。这里需要维护的实际上是窗口中的最大值和最小值，如果只需要维护最大或者最小，我们可以用优先队列来做，但这里需要同时维护两者，一般常用的就是`multiset`。窗口缩小的条件就是当最大最小值的差的绝对值比`limit`大，每次缩小完之后，维护一个最大的长度即可。

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

&emsp;&emsp;这道题目是非常简单的滑动窗口题目，提示很明显，限制条件也不难，如果对multiset不熟悉的话可能有点难度。这道题目的分享到这里，感谢你的支持！
