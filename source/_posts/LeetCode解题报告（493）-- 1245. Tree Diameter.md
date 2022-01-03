---
title: LeetCode解题报告（493）-- 1245. Tree Diameter
mathjax: true
date: 2022-01-02 23:20:17
---

## Problem

Given an undirected tree, return its diameter: the number of **edges** in a longest path in that tree.

The tree is given as an array of `edges` where `edges[i] = [u, v]` is a bidirectional edge between nodes `u` and `v`. Each node has labels in the set `{0, 1, ..., edges.length}`.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2019/06/14/1397_example_1.PNG)

```
Input: edges = [[0,1],[0,2]]
Output: 2
Explanation: 
A longest path of the tree is the path 1 - 0 - 2.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2019/06/14/1397_example_2.PNG)

```
Input: edges = [[0,1],[1,2],[2,3],[1,4],[4,5]]
Output: 4
Explanation: 
A longest path of the tree is the path 3 - 2 - 1 - 4 - 5.
```



**Constraints:**

- `0 <= edges.length < 10^4`
- `edges[i][0] != edges[i][1]`
- `0 <= edges[i][j] <= edges.length`
- The given edges form an undirected tree.

---

## Analysis

&emsp;&emsp;题目以边的形式给出一幅图，要求找到这幅图的最长的路径长度。题目只要返回长度，不需要把路径打印出来。假设我们最长的路径起点是`a`，终点是`b`，如果我们知道了`a`或者`b`中的其中一个，只用做一次BFS就能找到最长的长度。问题就转化为我们怎么找到最长路径的其中一个顶点。我们只需要任意找一个顶点，然后找到最长的路径，那个终点必定是最长路径的其中一个顶点，详细的证明请看https://stackoverflow.com/questions/20010472/proof-of-correctness-algorithm-for-diameter-of-a-tree-in-graph-theory。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int treeDiameter(vector<vector<int>>& edges) {
        // 2 time BFS
        n = edges.size() + 1;
        vector<vector<int>> graph(n);
        for (vector<int> &edge: edges) {
            graph[edge[0]].push_back(edge[1]);
            graph[edge[1]].push_back(edge[0]);
        }
        
        vector<int> v1 = bfs(0, graph);
        vector<int> v2 = bfs(v1[1], graph);
        return v2[0];
    }
private:
    int n;
    vector<int> bfs(int start, vector<vector<int>> &graph) {
        queue<int> q;
        q.push(start);
        vector<bool> visited(n, false);
        visited[start] = true;
        int count = 0, last = 0;
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; ++i) {
                int cur = q.front();
                q.pop();
                last = cur;
                for (int next: graph[cur]) {
                    if (!visited[next]) {
                        q.push(next);                        
                        visited[next] = true;
                    }
                }
            }
            ++count;
        }
        return {count - 1, last};
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难点就是如何找到最长路径的一个起点，具体的证明可以去stack overflow看，这里就算是一个总结了。coding上没有难度，就是两个一样的BFS。这道题目的分享到这里，感谢你的支持！

