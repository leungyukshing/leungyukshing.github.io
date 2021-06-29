---
title: LeetCode解题报告（75）-- 684. Redundant Connection
tags:
  - LeetCode
mathjax: true
abbrlink: 35576
date: 2019-11-19 10:52:33
---

## Problem

In this problem, a tree is an **undirected** graph that is connected and has no cycles.

The given input is a graph that started as a tree with N nodes (with distinct values 1, 2, ..., N), with one additional edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of `edges`. Each element of `edges` is a pair `[u, v]` with `u < v`, that represents an **undirected** edge connecting nodes `u` and `v`.

Return an edge that can be removed so that the resulting graph is a tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array. The answer edge `[u, v]` should be in the same format, with `u < v`.

<!-- more -->

**Example 1:**

```
Input: [[1,2], [1,3], [2,3]]
Output: [2,3]
Explanation: The given undirected graph will be like this:
  1
 / \
2 - 3
```

**Example 2:**

```
Input: [[1,2], [2,3], [3,4], [1,4], [1,5]]
Output: [1,4]
Explanation: The given undirected graph will be like this:
5 - 1 - 2
    |   |
    4 - 3
```

**Note:**

+ The size of the input 2D-array will be between 3 and 1000.

+ Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.

------

## Analysis

&emsp;&emsp;先理解一下题目要求，与图有关的题目。输入是一系列的节点对，表示这两个点之间有边相连。题目要求我们找到一条多余的边，去掉这条多余的边后，剩下的是一棵树。字面上有点难理解，通俗说就是让我们验证两个节点之间的连通性，如果这两个节点原本就已经连通，那么在这两个节点之间新增的这条边就是多余的，也就是我们要找到的边。

&emsp;&emsp;理解题目后，我们不难得出一个思路：对输入的节点对进行遍历，每次都验证这两个节点的连通性，如果这两个节点本来就已经连通，那么这个节点对就是我们要的答案。问题的难点来到了如何验证两个节点的连通性。

&emsp;&emsp;每增加一条边我们需要更新整个图的连通状态，这个问题我们称为**动态连通性问题**。而解决这个问题的神奇无疑是——**并查集**，一种兼具查找和合并的数据结构，专门用于解决动态连通性问题。下面是有关并查集的简单介绍，懂的朋友可以跳过。

&emsp;&emsp;我们先来看看并查集是如何动态维护一个图的连通性。我们对一个图中的节点进行分组，**相互连通的节点在同一个组**。我们可以使用一个map来存储，每个组都有一个唯一的标识key（一般使用号码最小的节点编号），map中每一个节点对应一个组号。对于查询这一步是非常快的，直接使用节点作为key来查询（时间复杂度$O(1)$）得到这个点所在的组，通过比对组号是否相同就能知道是否在同一个组。但是建立连通关系却比较麻烦，因为需要遍历全表。

&emsp;&emsp;我们期望的是一个高效的合并算法，传统的使用集合或者链表的方式肯定是行不通的，因为需要遍历就会需要$O(n)$级别的时间复杂度。取而代之，我们使用树作为描述集合的数据结构。在这个场景下，树有以下的好处：

1. 树的根节点是这棵树的leader，只要改变一个节点，就能够移动整一颗树；
2. 回溯，只要我们建立了合适的回溯机制，树的回溯比map的回溯高效得多。

&emsp;&emsp;整个算法思路如下：

1. 初始化：每一个节点都是一颗树，自己就是根节点
2. 查询：对于点对$(a,b)$，通过回溯分别找到$a$和$b$所在的类别，通过比对判断他们是否属于同一个组，从而判断连通性。
3. 合并：对于点对$(a,b)$，如果需要在这两个节点直接建立连接，我们就需要把他们所在的组合并。只需要把他们的父节点合并，合并后的组一般取较小的节点的组号。

&emsp;&emsp;回到这道题，实际上就是简单地应用并查集就能做出来。

------

## Solution

&emsp;&emsp;并查集的完整实现是比较复杂的，而且还有很多细节和优化的地方。这里从A题的角度出发，我只分享对于这一道题的做法。因为题目给出了节点的数量，我们不需要真正地构建一颗树。简单的做法是直接使用一个数组`root`记录每个节点的父节点。比如说第$i$号节点的根节点就是`root[i]`。这里的回溯直接下标访问数组就做到了，非常简单。

&emsp;&emsp;只要对于一个新的点对，我们发现他们已经连接，这个新的点对就是多余的，直接作为答案返回即可。

------

## Code

```c++
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int size = edges.size();
        int root[1001] = {};
        for (int i = 0; i < 1001; i++) {
            root[i] = i;
        }
        
        for (int i = 0; i < size; i++) {
            vector<int> cur = edges[i];
            int a = cur[0];
            int b = cur[1];
            if (findRoot(root, a) == findRoot(root, b)) {
                return cur;
            }
            root[findRoot(root, b)] = findRoot(root, a);
        }
        vector<int> temp;
        return temp;
    }
    
    int findRoot(int *root, int n) {
        if (root[n] == n) {
            return n;
        }
        root[n] = findRoot(root, root[n]);
        return root[n];
    }
};
```

------

## Summary

 &emsp;&emsp;借用这道题希望和大家分享有关并查集的内容。核心是理解并查集的想法，实现起来有很多简易的版本，需要结合实际情况加以改变。本题的分享到这里，欢迎大家转发、评论。最后，谢谢您的支持！