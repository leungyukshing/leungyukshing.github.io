---
title: LeetCode解题报告（357) -- 1642. Furthest Building You Can Reach
tags:
  - LeetCode
mathjax: true
date: 2021-05-01 23:06:45

---

## Problem

You are given an integer array `heights` representing the heights of buildings, some `bricks`, and some `ladders`.

You start your journey from building `0` and move to the next building by possibly using bricks or ladders.

While moving from building `i` to building `i+1` (**0-indexed**),

- If the current building's height is **greater than or equal** to the next building's height, you do **not** need a ladder or bricks.
- If the current building's height is **less than** the next building's height, you can either use **one ladder** or `(h[i+1] - h[i])` **bricks**.

*Return the furthest building index (0-indexed) you can reach if you use the given ladders and bricks optimally.*

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/10/27/q4.gif)

```
Input: heights = [4,2,7,6,9,14,12], bricks = 5, ladders = 1
Output: 4
Explanation: Starting at building 0, you can follow these steps:
- Go to building 1 without using ladders nor bricks since 4 >= 2.
- Go to building 2 using 5 bricks. You must use either bricks or ladders because 2 < 7.
- Go to building 3 without using ladders nor bricks since 7 >= 6.
- Go to building 4 using your only ladder. You must use either bricks or ladders because 6 < 9.
It is impossible to go beyond building 4 because you do not have any more bricks or ladders.
```

**Example 2:**

```
Input: heights = [4,12,2,7,3,18,20,3,19], bricks = 10, ladders = 2
Output: 7
```

**Example 3:**

```
Input: heights = [14,3,19,3], bricks = 17, ladders = 0
Output: 3
```

**Constraints:**

- `1 <= heights.length <= 105`
- `1 <= heights[i] <= 106`
- `0 <= bricks <= 109`
- `0 <= ladders <= heights.length`

------

## Analysis

&emsp;&emsp;题目给出一个数组，里面代表每栋楼的高度。还给出梯子和砖块的数量`ladders`和`bricks`。问从第0栋楼开始，最远可以移动到哪一栋楼。在楼之间移动的规则如下：

1. 如果下一栋楼比当前的楼矮，则不需要耗费任何的梯子或砖块；
2. 如果下一栋楼比当前的楼高，可以选择用梯子或者砖块。如果使用梯子，则消耗一个；如果使用砖块，砖块消耗的数量是两栋楼的高度差。

&emsp;&emsp; 这个移动规则其实已经有贪心算法的味道了，我们肯定希望把梯子用在高度差最大的地方，然后其他的地方就用砖块。我们可以先计算出每两栋楼之间的高度差，然后从前往后遍历。如果是负数，说明后面比前面的矮，不需要耗费，可以直接到下一个；如果是正数才需要处理。

&emsp;&emsp;因为我们要维护一个遍历到当前位置时，`bricks`个最大的高度差，这里很适合的数据结构就是堆。如果堆没有满则放进去，如果堆满了，就先把当前的高度差放进去，然后挑一个小的出堆，同时这也意味着需要耗费对应数量的砖块了。当砖块耗尽时，也就说明梯子肯定耗尽了，此时就可以结束遍历，返回当前位置作为结果。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int furthestBuilding(vector<int>& heights, int bricks, int ladders) {
        priority_queue<int, vector<int>, greater<int>> q;
        int size = heights.size();
        
        for (int i = 0; i < size - 1; i++) {
            int diff = heights[i + 1] - heights[i];
            if (diff < 0) {
                continue;
            }
            q.push(diff);
            while (q.size() > ladders) {
                if (bricks - q.top() < 0) {
                    return i;
                }
                bricks -= q.top();
                q.pop();
            }
        }
        return size - 1;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目核心还是选用堆来维护`bricks`个最大值，其余的难度不大。这道题目的分享到这里，感谢你的支持！
