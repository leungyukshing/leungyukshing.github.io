---
title: LeetCode解题报告（256）-- 84. Largest Rectangle in Histogram
tags:
  - LeetCode
mathjax: true
date: 2021-01-01 22:19:42

---

## Problem

Given *n* non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

<!-- more -->

![Example0](https://assets.leetcode.com/uploads/2018/10/12/histogram.png)

Above is a histogram where width of each bar is 1, given height = `[2,1,5,6,2,3]`.

![Example1](https://assets.leetcode.com/uploads/2018/10/12/histogram_area.png)

The largest rectangle is shown in the shaded area, which has area = `10` unit.

**Example:**

```
Input: [2,1,5,6,2,3]
Output: 10
```

------

## Analysis

&emsp;&emsp;这道题目要求直方图中最大的矩形面积。从主观上看，我们想要宽度比较大，同时高度也是比较高，这样最后计算出来的矩形面积就比较大。首先我们看什么时候才需要去计算，这里引入一个**局部峰值**的概念（和极大值、极小值类似）。只有在高度处于转折点的时候，才需要对前面的进行计算，这是为什么呢？比如说有四个矩形高度分别是1、2、3、1，那么我们只有当搜索到最后一个1的时候，发现3是局部峰值，这个时候才需要去计算前面的情况，而不需要在遇到1、2的时候就计算，因为这两个矩形的计算已经纳入到3的计算中。

&emsp;&emsp;所以基本的思路就是，每当我们遇到了一个局部峰值，都需要向前计算。每向前一个，就用当前的高度作为矩形的高度，再计算出长度，全局维护一个最优解即可。

------

## Solution

&emsp;&emsp;在确定局部峰值和向前计算这个问题的实现上，也是有技巧可言的。根据上面的分析，我们要找到局部峰值，比较方便的做法就是用一个栈去记录，当前元素比栈顶元素大的话就入栈，反之则开始向前处理。

------

## Code

```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> s;
        heights.push_back(0);
        int result = 0, size = heights.size();
        
        for (int i = 0; i < size; i++) {
            if (s.empty() || heights[i] > heights[s.top()]) {
                s.push(i);
            } else {
                int current = s.top();
                s.pop();
                result = max(result, heights[current] * (s.empty() ? i : i - s.top() - 1));
                i--;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较难的数组类型题目，它的巧妙之处是用了栈去维护局部峰值，然后向前处理。这道题这道题目的分享到这里，谢谢您的支持！
