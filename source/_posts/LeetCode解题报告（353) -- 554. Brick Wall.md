---
title: LeetCode解题报告（353) -- 554. Brick Wall
tags:
  - LeetCode
mathjax: true
abbrlink: 3617
date: 2021-04-27 21:32:19
---

## Problem

There is a rectangular brick wall in front of you with `n` rows of bricks. The `ith` row has some number of bricks each of the same height (i.e., one unit) but they can be of different widths. The total width of each row is the same.

Draw a vertical line from the top to the bottom and cross the least bricks. If your line goes through the edge of a brick, then the brick is not considered as crossed. You cannot draw a line just along one of the two vertical edges of the wall, in which case the line will obviously cross no bricks.

Given the 2D array `wall` that contains the information about the wall, return *the minimum number of crossed bricks after drawing such a vertical line*.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/04/24/cutwall-grid.jpg)

```
Input: wall = [[1,2,2,1],[3,1,2],[1,3,2],[2,4],[3,1,2],[1,3,1,1]]
Output: 2
```

**Example 2:**

```
Input: wall = [[1],[1],[1]]
Output: 3
```

**Constraints:**

- `n == wall.length`
- `1 <= n <= 104`
- `1 <= wall[i].length <= 104`
- `1 <= sum(wall[i].length) <= 2 * 104`
- `sum(wall[i])` is the same for each row `i`.
- `1 <= wall[i][j] <= 231 - 1`

------

## Analysis

&emsp;&emsp;题目给出一个二维数组，每一行里面有砖块的长度。要求找到一个竖直的线，从上往下切下来，穿过的砖块数量最少，问这个最少的数量是多少？

&emsp;&emsp;穿过的砖块数量最少，也意味着穿过的缝隙最多，这两句是等价的。所以我们可以找穿过的缝隙最多的位置。我们怎么去定义出这个缝隙呢？因为每块砖都知道长度，所以如果长度一样的话，那么他们结束的地方就会有一个共同的空隙。于是对每一行，我们把砖块的长度累加起来，每一个长度就代表这个长度后会有一个缝隙。这里用一个map存起来，因此key就是从头开始计算的长度，value就是这个长度之后出现缝隙的次数。我们不断去更新这个map，同时维护一个value的最大值，最后的结果就是总的行数减去这个极大值。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int leastBricks(vector<vector<int>>& wall) {
        map<int, int> m;
        int size = wall.size();
        int maxCount = 0;
        for (int i = 0; i < size; i++) {
            int s = wall[i].size();
            int temp = 0;
            for (int j = 0; j < s - 1; j++) {
                temp += wall[i][j];
                m[temp]++;
                maxCount = max(maxCount, m[temp]);
            }
        }
        return size - maxCount;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难度不算大，关键是有一个题意的转换，把找穿过最少的砖块，转化为找穿过最多的缝隙。这道题目的分享到这里，感谢你的支持！
