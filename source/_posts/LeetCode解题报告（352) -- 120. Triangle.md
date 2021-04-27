---
title: LeetCode解题报告（352) -- 120. Triangle
tags:
  - LeetCode
mathjax: true
date: 2021-04-26 19:03:04

---

## Problem

Given a `triangle` array, return *the minimum path sum from top to bottom*.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index `i` on the current row, you may move to either index `i` or index `i + 1` on the next row.

<!-- more -->

**Example 1:**

```
Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
Output: 11
Explanation: The triangle looks like:
   2
  3 4
 6 5 7
4 1 8 3
The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).
```

**Example 2:**

```
Input: triangle = [[-10]]
Output: -10
```

**Constraints:**

- `1 <= triangle.length <= 200`
- `triangle[0].length == 1`
- `triangle[i].length == triangle[i - 1].length + 1`
- `-104 <= triangle[i][j] <= 104`

 

**Follow up:** Could you do this using only `O(n)` extra space, where  is the total number of rows in the triangle?

------

## Analysis

&emsp;&emsp;题目给出一个三角形，要求找出一条从上到下的路径，使得这条路径上的值之和最小，只需要返回最小值而不用返回路径。题目的描述指向性很明显，就是dp了，但是怎么做dp是这道题目的难点。

&emsp;&emsp;我们还是使用二维数组来存放dp，定义`dp[i][j]`为从顶部到位置`[i,j]`的最小路径。每个位置的最小路径，由两个位置的值决定，一个是其左上方`[i - 1, j]`，一个是其右上方`[i - 1, j + 1]`，所以只需要在这两个位置取较小的一个，加上当前位置的值即可。状态转移方程为`dp[i][j] = min(dp[i - 1][j] + dp[i - 1][j + 1]) + triangle[i][j]`。边界情况直接处理即可。

&emsp;&emsp;上面这种方法已经实现了dp的解法，但是题目的Follow up提到能否优化空间呢？再看回上面的过程，实际上每一层的结果，只和他的上一层有关系，所以当处理到第三层的时候，其实第一层的值已经没有用的，这就说明存储空间是可以压缩的。我们可以从下往上走，方向转变后，其实最小值的计算并没有不同，还是取下层左右两边元素的极小值，再加上当前元素的值。一直往上计算后，最后只剩下一个元素，这个就是最小路径的值。

------

## Solution

&emsp;&emsp;这道题目的难度在于压缩空间，从上往下计算当然也是可以的，但是因为空间原因，代码量要比从下往上要大。同时还需要处理好边界条件。

------

## Code

```c++
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        vector<int> dp(triangle.back());
        for (int i = (int)triangle.size() - 2; i >= 0; --i) {
            for (int j = 0; j <= i; ++j) {
                dp[j] = min(dp[j], dp[j + 1]) + triangle[i][j];
            }
        }
        return dp[0];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是难度中等的dp类题目，而且还增加了压缩空间的考点。总的来说思路是传统的，但是结合到空间压缩，就需要考虑dp转移的方向。这道题目的分享到这里，感谢你的支持！
