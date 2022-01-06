---
title: LeetCode解题报告（501）-- 1135. Connecting Cities With Minimum Cost
mathjax: true
date: 2022-01-05 21:53:39
---

## Problem

There are `n` cities labeled from `1` to `n`. You are given the integer `n` and an array `connections` where `connections[i] = [xi, yi, costi]` indicates that the cost of connecting city `xi` and city `yi` (bidirectional connection) is `costi`.

Return *the minimum **cost** to connect all the* `n` *cities such that there is at least one path between each pair of cities*. If it is impossible to connect all the `n` cities, return `-1`,

The **cost** is the sum of the connections' costs used.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2019/04/20/1314_ex2.png)

```
Input: n = 3, connections = [[1,2,5],[1,3,6],[2,3,1]]
Output: 6
Explanation: Choosing any 2 edges will connect all cities so we choose the minimum 2.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2019/04/20/1314_ex1.png)

```
Input: n = 4, connections = [[1,2,3],[3,4,4]]
Output: -1
Explanation: There is no way to connect all cities even if all edges are used.
```



**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 1000`
- `0 <= matrix[i][j] <= 106`
- `1 <= k <= m * n`

---

## Analysis

&emsp;&emsp;这道题目也是以边的方式给出了一个图，边上是有cost的，问最少的cost去连接整幅图是多少？这个实际上就是最小生成树的方法，所以我就通过这道题目去分享下如何写一个MST算法。

&emsp;&emsp;MST算法有很多，这里分享的是叫**Kruskal's Algorithm**。其原理很简单，首先对所有的边按照cost从小到大排列（本质上就是一种greedy algorithm），然后从头往后遍历，如果新增当前这条边能够连接一个新的节点，我们就使用这条边，如果新增的这条边不能新增一个节点，我们就忽略。那么这里我们要快速判断，新增的边能否新增节点，要怎么做呢？这实际上就是一个动态连通性的问题，用并查集。一开始所有的点独自是一个连通分量，通过边把两个点连起来，然后通过并查集查找两个点的关系，如果是合并成一个连通分量，那么说明这条边是需要的，加上cost；否则忽略这条边。

&emsp;&emsp;最后别忘了处理不能连接所有节点的情况，我们可以在并查集的class中添加一个`count`表示连通分量的数量，这样就可以快速判断了。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class DSU {
public:
    DSU(int N) {
        parent = vector<int>(N, -1);
        rank = vector<int>(N, 1);
        for (int i = 0; i < N; ++i) {
            parent[i] = i;
        }
        count = N;
    }
    
    int find(int x) {
        while (x != parent[x]) {
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x;
    }
    
    void unio(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX == rootY) {
            return;
        }
        
        if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
            rank[rootX] += rank[rootY];
        } else {
            parent[rootX] = rootY;
            rank[rootY] += rank[rootX];
        }
        --count;
    }
    
    bool isConnected(int x, int y) {
        return find(x) == find(y);
    }
    
    int getCount() {
        return count;
    }
private:
    int count; // number of components
    vector<int> parent;
    vector<int> rank;
};

class Solution {
public:
    int minimumCost(int n, vector<vector<int>>& connections) {
        sort(connections.begin(), connections.end(), [](const vector<int> &a, const vector<int> &b) {
            return a[2] < b[2];
        });
        
        DSU dsu(n);
        
        int result = 0;
        for (vector<int> &connection: connections) {
            int a = connection[0] - 1;
            int b = connection[1] - 1;
        
            if (dsu.isConnected(a, b)) {
                //cout << a << " and " << b << " are algready connected" << endl;
                continue;
            }
            
            //cout << "connect " << a << " and " << b << " with cost " << connection[2] << endl;
            
            dsu.unio(a, b);
            result += connection[2];
        }
        
        return dsu.getCount() == 1 ? result: -1;
    }
};
```

------

## Summary

&emsp;&emsp;借用这道题目我们学会了MST算法中的一种，其实也就是基于并查集做的一个贪心算法。这道题目的分享到这里，感谢你的支持！

