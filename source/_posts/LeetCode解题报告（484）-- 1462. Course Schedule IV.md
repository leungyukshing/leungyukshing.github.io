---
title: LeetCode解题报告（484）-- 1462. Course Schedule IV
mathjax: true
date: 2022-01-01 13:42:38
---

## Problem

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `ai` first if you want to take course `bi`.

- For example, the pair `[0, 1]` indicates that you have to take course `0` before you can take course `1`.

Prerequisites can also be **indirect**. If course `a` is a prerequisite of course `b`, and course `b` is a prerequisite of course `c`, then course `a` is a prerequisite of course `c`.

You are also given an array `queries` where `queries[j] = [uj, vj]`. For the `jth` query, you should answer whether course `uj` is a prerequisite of course `vj` or not.

Return *a boolean array* `answer`*, where* `answer[j]` *is the answer to the* `jth` *query.*

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/05/01/courses4-1-graph.jpg)

```
Input: numCourses = 2, prerequisites = [[1,0]], queries = [[0,1],[1,0]]
Output: [false,true]
Explanation: The pair [1, 0] indicates that you have to take course 1 before you can take course 0.
Course 0 is not a prerequisite of course 1, but the opposite is true.
```

**Example 2:**

```
Input: numCourses = 2, prerequisites = [], queries = [[1,0],[0,1]]
Output: [false,false]
Explanation: There are no prerequisites, and each course is independent.
```

**Example 3:**

![Example3](https://assets.leetcode.com/uploads/2021/05/01/courses4-3-graph.jpg)

```
Input: numCourses = 3, prerequisites = [[1,2],[1,0],[2,0]], queries = [[1,0],[1,2]]
Output: [true,true]
```



**Constraints:**

- `2 <= numCourses <= 100`
- `0 <= prerequisites.length <= (numCourses * (numCourses - 1) / 2)`
- `prerequisites[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`
- All the pairs `[ai, bi]` are **unique**.
- The prerequisites graph has no cycles.
- `1 <= queries.length <= 104`
- `0 <= ui, vi <= n - 1`
- `ui != vi`

---

## Analysis

&emsp;&emsp;这是课程表系列题的第4题，背景还是一样的，不同的是prerequisite的关系是有传递性的，同时题目还给出了`queries`，每个query`[i, j]`查询`i`是否`j`的prerequisite。对于这种query类型的题目，我们的处理思路一般是求解出所有的答案，然后直接返回，一般是求解出一个矩阵便于快速访问。这里就需要求解出`isReach[i][j]`代表`i`是否`j`的prerequisite。我们以每个节点作为BFS的起点都走一遍，就能构造出`isReach`了。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    vector<bool> checkIfPrerequisite(int numCourses, vector<vector<int>>& prerequisites, vector<vector<int>>& queries) {
        // build graph
        vector<vector<int>> graph(numCourses);
        
        for (vector<int> &pre: prerequisites) {
            graph[pre[0]].push_back(pre[1]);
        }
        
        vector<vector<bool>> isReach(numCourses, vector<bool>(numCourses, false));
        
        for (int i = 0; i < numCourses; ++i) {
            queue<int> q;
            q.push(i);
            
            while (!q.empty()) {
                int size = q.size();
                for (int k = 0; k < size; ++k) {
                    int cur = q.front();
                    q.pop();
                    
                    for (int next: graph[cur]) {
                        if (isReach[i][next]) {
                            continue;
                        }
                        isReach[i][next] = true;
                        q.push(next);
                    }
                }
            }
        }
        
        vector<bool> result;
        for (vector<int> &query: queries) {
            result.push_back(isReach[query[0]][query[1]]);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目就是多次BFS，没有特别的难点，主要是根据`queries`这种题目特点确定出要计算出一个`isReach`二维数组。这道题目的分享到这里，感谢你的支持！

