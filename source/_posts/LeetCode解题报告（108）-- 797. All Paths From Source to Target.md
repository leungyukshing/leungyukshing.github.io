---
title: LeetCode解题报告（108）-- 797. All Paths From Source to Target
tags:
  - LeetCode
date: 2020-07-26 10:52:20
mathjax: true

---

## Problem

Given a directed, acyclic graph of `N` nodes.  Find all possible paths from node `0` to node `N-1`, and return them in any order.

The graph is given as follows:  the nodes are 0, 1, ..., graph.length - 1.  graph[i] is a list of all nodes j for which the edge (i, j) exists.

<!-- more -->

**Example:**

```
Example:
Input: [[1,2], [3], [3], []] 
Output: [[0,1,3],[0,2,3]] 
Explanation: The graph looks like this:
0--->1
|    |
v    v
2--->3
There are two paths: 0 -> 1 -> 3 and 0 -> 2 -> 3.
```

**Note:**

- The number of nodes in the graph will be in the range `[2, 15]`.
- You can print different paths in any order, but you should keep the order of nodes inside one path.

------

## Analysis

&emsp;&emsp;这道题目是属于比较简单的图遍历问题，题目给出邻接矩阵，要求寻找从0到`n-1`的路径。因为source和target都是确定的，所以起始和终止条件都是确定的，起始都是0号点，终止就是判断数字是否`size-1`。中间的遍历过程就是每到一个点，就递归遍历它所有邻接的点。

------

## Solution

&emsp;&emsp;递归的过程把所有的信息都带上，包括整个图，当前路径和所有答案，还要当前遍历到的节点的编号，因为是无环图，所以不需要记录`visited`。

------

## Code

```c++
class Solution {
public:
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        vector<vector<int>> res;
        helper(graph, 0, {}, res);
        return res;
    }
    
    void helper(vector<vector<int>>& graph, int cur, vector<int> path, vector<vector<int>>& res) {
        path.push_back(cur);
        
        if (cur == graph.size() - 1) {
            res.push_back(path);
        }
        else {
            for (int neigh : graph[cur]) {
                helper(graph, neigh, path, res);
            }
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目属于图遍历的简单题，也不需要构图，所以做起来还是比较快的。需要注意的是递归是处理图问题常用的方法，递归时要把所有信息都带上。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
