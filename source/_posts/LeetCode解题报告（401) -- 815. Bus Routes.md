---
title: LeetCode解题报告（401) -- 815. Bus Routes
mathjax: true
date: 2021-09-04 14:50:11
---

## Problem

You are given an array `routes` representing bus routes where `routes[i]` is a bus route that the `ith` bus repeats forever.

- For example, if `routes[0] = [1, 5, 7]`, this means that the `0th` bus travels in the sequence `1 -> 5 -> 7 -> 1 -> 5 -> 7 -> 1 -> ...` forever.

You will start at the bus stop `source` (You are not on any bus initially), and you want to go to the bus stop `target`. You can travel between bus stops by buses only.

Return *the least number of buses you must take to travel from* `source` *to* `target`. Return `-1` if it is not possible.

<!-- more -->

**Example 1:**

```
Input: routes = [[1,2,7],[3,6,7]], source = 1, target = 6
Output: 2
Explanation: The best strategy is take the first bus to the bus stop 7, then take the second bus to the bus stop 6.
```

**Example 2:**

```
Input: routes = [[7,12],[4,5,15],[6],[15,19],[9,12,13]], source = 15, target = 12
Output: -1
```

**Constraints:**

- `1 <= routes.length <= 500`.
- `1 <= routes[i].length <= 105`
- All the values of `routes[i]` are **unique**.
- `sum(routes[i].length) <= 105`
- `0 <= routes[i][j] < 106`
- `0 <= source, target < 106`

------

## Analysis

&emsp;&emsp;还是一道hard，这道是比较著名的公交车问题。题目给出若干条线路，每条线路包含若干个车站。我们要计算的是从`source`到`target`的公交车线路次数，因此我们把线路当作节点，这样就能转换成节点之间的距离问题。那么节点之间的距离怎么确定呢？答案是**换乘车站**。两条公交线路如果有相同的站，那么乘客就能在该站换成，从线路A换成到线路B，相当于在节点A和节点B之间添加了一条长度为1的边。因此整个构图的思路我们就确定了。

&emsp;&emsp;有了这个构图的思路，解题就变得容易。我们只需要从`source`开始进行BFS，最终看到`target`最短的距离就可以了。但是这里还有一个坑，由于我们是从某个站出发，到达某个站，而一个站可以位于多条线路上，所以这里的BFS需要分开，要把`source`、BFS和`target`三部分分开处理。

&emsp;&emsp;我们用一个hash map记录站点和线路的关系，这是用于构建邻接矩阵的，也是用来确定`source`和`target`到底在哪些线路上（哪些节点）。整个图我们还是采用传统的二维矩阵来存储，借助上面的hash map，如果同一个站有多条线路，那么这些线路的节点就需要关联起来。接着是对`source`的处理，我们要遍历`source`所在的所有路线（节点），然后把这些节点都扔到队列中。然后就是BFS了，每次把队头元素取出（这个指的是线路，也就是节点），然后到二维矩阵中去寻找相连的其他节点（通过`distance[i]`是否为-1来判断这个节点之前是否被访问过），找到后更新距离，并把新的节点放入到队列中。最后是处理`target`，我们需要遍历`target`所在的所有线路（节点），然后找出距离的最小值。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int numBusesToDestination(vector<vector<int>>& routes, int source, int target) {
        if (source == target) {
            return 0;
        }
        int size = routes.size();
        map<int, vector<int>> m; // site->line
        vector<vector<bool>> exchange(size, vector<bool>(size)); // line
        
        for (int i = 0; i < size; i++) {
            for (int site: routes[i]) {
                for (int j: m[site]) {
                    exchange[i][j] = exchange[j][i] = true;
                }
                m[site].push_back(i);
            }
        }
        
        vector<int> distance(size, -1);
        queue<int> q;
        for (int bus: m[source]) {
            distance[bus] = 1;
            q.push(bus);
        }
        
        while (!q.empty()) {
            int cur = q.front();
            q.pop();
            for (int x = 0; x < size; x++) {
                if (exchange[cur][x] && distance[x] == -1) {
                    distance[x] = distance[cur] + 1;
                    q.push(x);
                }
            }
        }
        
        int result = INT_MAX;
        for (int bus: m[target]) {
            if (distance[bus] != -1) {
                result = min(result, distance[bus]);
            }
        }
        return result == INT_MAX? -1: result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是一道经典的图BFS题目，BFS的过程比较常规，但是从题目转化为图这个是难点。需要把线路抽象为节点，然后利用相同的站点构建图，这是比较考验大家的做题直觉的。这道题目的分享到这里，感谢你的支持！
