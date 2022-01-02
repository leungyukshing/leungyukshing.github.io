---
title: LeetCode解题报告（492）-- 1466. Reorder Routes to Make All Paths Lead to the City Zero
mathjax: true
date: 2022-01-01 23:31:28
---

## Problem

There are `n` cities numbered from `0` to `n - 1` and `n - 1` roads such that there is only one way to travel between two different cities (this network form a tree). Last year, The ministry of transport decided to orient the roads in one direction because they are too narrow.

Roads are represented by `connections` where `connections[i] = [ai, bi]` represents a road from city `ai` to city `bi`.

This year, there will be a big event in the capital (city `0`), and many people want to travel to this city.

Your task consists of reorienting some roads such that each city can visit the city `0`. Return the **minimum** number of edges changed.

It's **guaranteed** that each city can reach city `0` after reorder.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/05/13/sample_1_1819.png)

```
Input: n = 6, connections = [[0,1],[1,3],[2,3],[4,0],[4,5]]
Output: 3
Explanation: Change the direction of edges show in red such that each node can reach the node 0 (capital).
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/05/13/sample_2_1819.png)

```
Input: n = 5, connections = [[1,0],[1,2],[3,2],[3,4]]
Output: 2
Explanation: Change the direction of edges show in red such that each node can reach the node 0 (capital).
```

**Example 3:**

```
Input: n = 3, connections = [[1,0],[2,0]]
Output: 0
```



**Constraints:**

- `2 <= n <= 5 * 104`
- `connections.length == n - 1`
- `connections[i].length == 2`
- `0 <= ai, bi <= n - 1`
- `ai != bi`

---

## Analysis

&emsp;&emsp;题目以边的方式给出了一个**有向图**，要求通过更改某些边的方向，使得所有城市都能到达city0，且更改的数量是最少的。首先因为要更改的数量是最少的，所以当然就是要在最短路径上找了，在一副无向图中找每个点到city0的最短路径这个不难，一个BFS搞定。问题是怎么在有向图中找并且要计算翻转边的数量。首先我们可以把所有的边都看作是双向的，但是用正负值区分开方向，比如`graph[i] = {j}`大于0说明有一条`i`指向`j`的边，如果`graph[i] = {j}`小于0说明有一条`j`指向`i`的边，如果是0就是不存在边了。

&emsp;&emsp;然后我们开始遍历，如果不为0说明是可以连接的，如果是正数说明这条边需要被翻转，因为我们是从city0出发，而题目要求的是其他城市到city0，方向刚好相反。我们只需要记录正数边的数量即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int minReorder(int n, vector<vector<int>>& connections) {
        vector<vector<int>> graph(n); // positive: forward; negative: backward
        for (vector<int> &connection: connections) {
            graph[connection[0]].push_back(connection[1]);
            graph[connection[1]].push_back(-connection[0]);
        }
        
        queue<int> q;
        vector<bool> visited(n, false);
        q.push(0);
        visited[0] = true;
        int result = 0;
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                int cur = q.front();
                q.pop();
                
                for (int next: graph[cur]) {
                    if (visited[abs(next)]) {
                        continue;
                    }
                    if (next > 0) {
                        ++result;
                    }
                    visited[abs(next)] = true;
                    q.push(abs(next));
                }
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的本质还是图的BFS，只不过在方向上加了点难度，通过正负值区分方向是一种很常见的手段。这道题目的分享到这里，感谢你的支持！

