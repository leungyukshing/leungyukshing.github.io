---
title: LeetCode解题报告（76）-- 947. Most Stones Removed with Same Row or Column
tags:
  - LeetCode
mathjax: true
abbrlink: 34312
date: 2019-11-24 21:46:30
---

## Problem

On a 2D plane, we place stones at some integer coordinate points.  Each coordinate point may have at most one stone.

Now, a *move* consists of removing a stone that shares a column or row with another stone on the grid.

What is the largest possible number of moves we can make?

<!-- more -->

**Example 1:**

```
Input: stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
Output: 5
```

**Example 2:**

```
Input: stones = [[0,0],[0,2],[1,1],[2,0],[2,2]]
Output: 3
```

**Example 3:**

```
Input: stones = [[0,0]]
Output: 0
```

**Note:**

1. `1 <= stones.length <= 1000`
2. `0 <= stones[i][j] < 10000`

------

## Analysis

&emsp;&emsp;先理解一下题目，在二维平面上我们放置石头，定义一次**move**为：如果一个石头的横坐标或纵坐标与另一个石头相同，那么移除这个石头就被成为一次**move**。从另一个角度来看，我们可以这样理解，如果一个石头的横坐标或纵坐标与另一个石头相同，那么我们认为他们之间存在链接。同一个连通分支中，通过**move**最后剩下的肯定只有一个，所以我们要求的答案就是总的石头数-连通分支的数量。

&emsp;&emsp;为什么我要转换理解的角度呢？因为一旦转化为动态连接的问题，我们就能够很方便地使用并查集来解决。在之前的题目中，我们尝试过使用最简单的并查集去解决动态连通的问题。同样的思路，我们应用到这道题，逐个遍历每个点，动态监测他们的连通性，维护一个根节点的列表，最后统计这个列表的数目就可以了。但是这道题我们关注的对象是二维平面上的点，我们不方便直接地表示某个点（当然，简单粗暴的方式是给每个点一个编号）。这里有一种更加优美的方法。

&emsp;&emsp;我们需要稍作变化，以提高解题效率。只要理解了连通的思路，我们就知道，这道题我们实际上是对每个点的横纵坐标进行连接，也就是说，对于某两个点，我们并不关注他们是通过横坐标连接还是通过纵坐标连接的，我们只需要知道他们是连接的就可以了，因此我们可以对节点进行降维处理，将纵坐标转化为横坐标，统一进行一维的处理。这里利用题目给出的限定，坐标的大小不超过10000，因此我们可以对纵坐标进行+10000的处理（相当于将纵坐标映射到横坐标后面），这样我们就能够在一维的角度去处理连接问题了。

------

## Solution

&emsp;&emsp;新建一个vector记录左右的坐标点，因为有降维处理，所以大小为20000。然后遍历每一个石头，分别查询他们的横坐标和纵坐标的根节点，如果相同则不需要新建连接，如果不同则建立连接（使用横坐标或纵坐标作为根节点都可以，代码中我使用了横坐标）。最后使用set来统计各个石头的根节点并去重，set的大小就是连通分支数。

------

## Code

```c++
class Solution {
public:
    int removeStones(vector<vector<int>>& stones) {
        if (stones.size() <= 1) {
            return 0;
        }
        int size = stones.size();
        vector<int> points(20000, -1);
        
        for (int i = 0; i < size; i++) {
            toOneDim(points, stones[i][0], stones[i][1] + 10000);
        }
        set<int> parents;
        for (int i = 0; i < size; i++) {
            parents.insert(findParent(points, stones[i][0]));
        }
        return size - parents.size();
    }
private:
    int findParent(vector<int> &points, int x) {
        if (points[x] == -1)
            return x;
        return findParent(points, points[x]);
    }
    void toOneDim(vector<int> &points, int x, int y) {
        int px = findParent(points, x);
        int py = findParent(points, y);
        if (px != py) {
            // not the same parents, add new connection
            points[py] = px;
        }
    }
};
```

------

## Summary

 &emsp;&emsp;这道题是第二次使用并查集解决的题目。我们需要将题目抽象并转化为动态连接问题，然后决定使用并查集完成。同时，为了编码的简洁，在充分理解题目的基础上，可以做适当地优化，比如这道题的降维处理。这道题的分享到这里结束了，欢迎转发、评论，谢谢！