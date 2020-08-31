---
title: LeetCode解题报告（119）-- 952. Largest Component Size by Common Factor
tags:
  - LeetCode
mathjax: true
date: 2020-08-31 11:39:19

---

## Problem

Given a non-empty array of unique positive integers `A`, consider the following graph:

- There are `A.length` nodes, labelled `A[0]` to `A[A.length - 1];`
- There is an edge between `A[i]` and `A[j]` if and only if `A[i]` and `A[j]` share a common factor greater than 1.

Return the size of the largest connected component in the graph.

<!-- more -->

**Example 1:**

```
Input: [4,6,15,35]
Output: 4
```

![Example1](https://assets.leetcode.com/uploads/2018/12/01/ex1.png)

**Example 2:**

```
Input: [20,50,9,63]
Output: 2
```

![Example2](https://assets.leetcode.com/uploads/2018/12/01/ex2.png)

**Example 3:**

```
Input: [2,3,6,7,4,12,21,39]
Output: 8
```

![Example3](https://assets.leetcode.com/uploads/2018/12/01/ex3.png)

**Note:**

1. `1 <= A.length <= 20000`
2. `1 <= A[i] <= 100000`

------

## Analysis

&emsp;&emsp;题目给出了若干个数，然后定义两个数相连的条件是他们有大于1的公因子。之前也遇到过类似的**动态连接问题**，当时的解决方法是用并查集，这里也是一样的，但是用法有点不同。这里我们怎么去判断某个节点的类别呢？

&emsp;&emsp;因为是通过公因子来建立连接关系的，所以我们直接就用公因子作为某个数的类别。以15为例，它的公因子（1和本身除外）就是3、5。所以它就有两个类别。如果有相同的公因子，那么就把他们连接起来。构建完并查集后，我们遍历每个数字的父节点（类别），最后找到数量最多的那个类别，这个数量的就是连通分量的数量了。

------

## Solution

&emsp;&emsp;这道题最本质的就是并查集的实现。用数组中最大的数作为最大的节点号码，然后初始化整个并查集。并查集的`find`和`union`操作都比较简单。然后就是遍历题目给出的数字，找到因子。这里先计算sqrt，可以减少遍历的次数。最后用map来统计。

------

## Code

```c++
class DSU {
private:
    vector<int> p;
public:
    DSU(int n) {
        for (int i = 0; i < n; i++) {
            p.push_back(i);
        }
    }
    int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
    
    void u(int x, int y) {
        p[find(x)] = p[find(y)];
    }
};

class Solution {
public:
    int largestComponentSize(vector<int>& A) {
        int n = *max_element(begin(A), end(A));
        int size = A.size();
        DSU dsu(n + 1);
        
        // construct DSU
        for (int i = 0; i < size; i++) {
            int t = sqrt(A[i]);
            for (int k = 2; k <= t; k++) {
                if (A[i] % k == 0) {
                    dsu.u(A[i], A[i] / k);
                    dsu.u(A[i], k);
                }
            }
        }
        
        // find the largest one
        unordered_map<int, int> m;
        int result = 1;
        for (int i = 0; i < size; i++) {
            result = max(result, ++m[dsu.find(A[i])]);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是并查集很好的一个应用例子，之前的题目都是并查集的直接使用，这道题需要结合题目的特点，去调整并查集的使用，但是背后使用的本质还是连通性的维持。这道题的分析到这里，谢谢！
