---
title: LeetCode解题报告（441）-- 1499. Max Value of Equation
mathjax: true
date: 2021-12-23 14:12:15
---

## Problem

You are given an array `points` containing the coordinates of points on a 2D plane, sorted by the x-values, where `points[i] = [xi, yi]` such that `xi < xj` for all `1 <= i < j <= points.length`. You are also given an integer `k`.

Return *the maximum value of the equation* `yi + yj + |xi - xj|` where `|xi - xj| <= k` and `1 <= i < j <= points.length`.

It is guaranteed that there exists at least one pair of points that satisfy the constraint `|xi - xj| <= k`.

<!-- more -->

**Example 1:**

```
Input: points = [[1,3],[2,0],[5,10],[6,-10]], k = 1
Output: 4
Explanation: The first two points satisfy the condition |xi - xj| <= 1 and if we calculate the equation we get 3 + 0 + |1 - 2| = 4. Third and fourth points also satisfy the condition and give a value of 10 + -10 + |5 - 6| = 1.
No other pairs satisfy the condition, so we return the max of 4 and 1.
```

**Example 2:**

```
Input: points = [[0,0],[3,0],[9,2]], k = 3
Output: 3
Explanation: Only the first two points have an absolute difference of 3 or less in the x-values, and give the value of 0 + 0 + |0 - 3| = 3.
```

**Constraints:**

- `2 <= points.length <= 105`
- `points[i].length == 2`
- `-108 <= xi, yi <= 108`
- `0 <= k <= 2 * 108`
- `xi < xj` for all `1 <= i < j <= points.length`
- `xi` form a strictly increasing sequence.

------

## Analysis

&emsp;&emsp;题目给出一个方程$y_i + y_j + |x_i - x_j|$，要求使得这个方程最大，同时又有一个限制条件$|x_i - x_j| <= k$。看上去很多变量，比较难入手。这里我先对方程进行初步处理，因为题目给出的`points`是已经按照`x-value`排好序的，所以$x_i < x_j$，因此上面的方程可以变为$y_i + y_j + x_j - x_i = (x_j + y_j) + (y_i - x_i)$。这样做的好处是可以分开考虑两个`point`的值。为了使得这个方程的值最大，我们希望$y_i - x_i$最大，因此我选择使用优先队列去维护这个值。同时因为有限制条件，所以优先队列中存一个`pair`，内容是$[y_i - x_i, x_i]$。

&emsp;&emsp;每次都和队头比较，如果$|x_j - x_i|$不符合条件，则把队头pop出，继续处理下一个。把不符合条件的点都去除掉之后，如果队列不为空说明可以计算一下方程的值，这里全局维护一个极大值即可。最后把自己放入到队列中，给后面的点计算。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
struct cmp {
    bool operator()(const pair<int, int> &a, const pair<int, int> &b) {
        if (a.first == b.first) {
            return a.second < b.second;
        }
        return a.first < b.first;
    }
};
class Solution {
public:
    int findMaxValueOfEquation(vector<vector<int>>& points, int k) {
        priority_queue<pair<int, int>, vector<pair<int, int>>, cmp> pq;
        int result = INT_MIN;
        for (vector<int> &point: points) {
            while (!pq.empty() && abs(pq.top().second - point[0]) > k) {
                pq.pop();
            }
            if (!pq.empty()) {
                result = max(result, point[0] + point[1] + pq.top().first);
            }
            pq.push({point[1] - point[0], point[0]});
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目编程上不难，比较困难的是化简方程，把两个点的计算分离开，然后通过利用优先队列去达到求最大值的目的。这道题目的分享到这里，感谢你的支持！
