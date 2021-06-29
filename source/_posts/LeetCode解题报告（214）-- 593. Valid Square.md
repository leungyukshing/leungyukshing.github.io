---
title: LeetCode解题报告（214）-- 593. Valid Square
tags:
  - LeetCode
mathjax: true
abbrlink: 54193
date: 2020-11-12 00:26:12
---

## Problem

Given the coordinates of four points in 2D space, return whether the four points could construct a square.

The coordinate (x,y) of a point is represented by an integer array with two integers.

<!-- more -->

**Example:**

```
Input: p1 = [0,0], p2 = [1,1], p3 = [1,0], p4 = [0,1]
Output: True
```

Note:

1. All the input integers are in the range [-10000, 10000].
2. A valid square has four equal sides with positive length and four equal angles (90-degree angles).
3. Input points have no order.

------

## Analysis

&emsp;&emsp;这是一道计算几何的编程题，这类题目考查的更多的是几何方面的知识。题目给出四个点的坐标，要求判断这四个点能否构成一个**正方形**。正方形最大的特点就是**四条边相等，对角线相等**，我们就是利用这个性质去判断的。

&emsp;&emsp;通过计算四个点之间的距离，如果是正方形的话，那么计算出来的结果只会有两种，一种是边长的长度，另一种是对角线的长度。最后我们判断计算结果有多少种就可以知道能否构成正方形了。

------

## Solution

&emsp;&emsp;因为我们需要记录的是不同长度的个数，使用set或者map即可。**特别需要注意，如果计算出来两点之间的距离是0的话，说明两个点重叠了，这种情况直接return false**。

------

## Code

```c++
class Solution {
public:
    bool validSquare(vector<int>& p1, vector<int>& p2, vector<int>& p3, vector<int>& p4) {
        map<int, int> m;
        vector<vector<int>> v{p1, p2, p3, p4};
        
        for (int i = 0; i < 4; i++) {
            for (int j = i + 1; j < 4; j++) {
                int x1 = v[i][0], y1 = v[i][1], x2 = v[j][0], y2 = v[j][1];
                int distance = pow((x1 - x2), 2) + pow((y1 - y2), 2);
                if (distance == 0) {
                    return false;
                }
                m[distance]++;
            }
        }
        return m.size() == 2;
    }
};
```

------

## Summary

&emsp;&emsp;计算几何的题目大多数比较难临场发挥，因为他对coding的要求并不高，反而是要求大家能够掌握一些基本的数学知识。因此在碰到这类型题目的时候就要善于总结下来，做好准备。这道题目的分享到这里，谢谢！
