---
title: LeetCode解题报告（232）-- 218. The Skyline Problem
tags:
  - LeetCode
mathjax: true
abbrlink: 56555
date: 2020-11-30 23:44:27
---

## Problem

A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are **given the locations and height of all the buildings** as shown on a cityscape photo (Figure A), write a program to **output the skyline** formed by these buildings collectively (Figure B).

<!-- more -->

![pic1](https://assets.leetcode.com/uploads/2018/10/22/skyline1.png)

![pic2](https://assets.leetcode.com/uploads/2018/10/22/skyline2.png)

The geometric information of each building is represented by a triplet of integers `[Li, Ri, Hi]`, where `Li` and `Ri` are the x coordinates of the left and right edge of the ith building, respectively, and `Hi` is its height. It is guaranteed that `0 ≤ Li, Ri ≤ INT_MAX`, `0 < Hi ≤ INT_MAX`, and `Ri - Li > 0`. You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.

For instance, the dimensions of all buildings in Figure A are recorded as: `[ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ] `.

The output is a list of "**key points**" (red dots in Figure B) in the format of `[ [x1,y1], [x2, y2], [x3, y3], ... ]` that uniquely defines a skyline. **A key point is the left endpoint of a horizontal line segment**. Note that the last key point, where the rightmost building ends, is merely used to mark the termination of the skyline, and always has zero height. Also, the ground in between any two adjacent buildings should be considered part of the skyline contour.

For instance, the skyline in Figure B should be represented as:`[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ]`.

**Notes:**

- The number of buildings in any input list is guaranteed to be in the range `[0, 10000]`.
- The input list is already sorted in ascending order by the left x position `Li`.
- The output list must be sorted by the x position.
- There must be no consecutive horizontal lines of equal height in the output skyline. For instance, `[...[2 3], [4 5], [7 5], [11 5], [12 7]...]` is not acceptable; the three lines of height 5 should be merged into one in the final output as such: `[...[2 3], [4 5], [12 7], ...]`

------

## Analysis

&emsp;&emsp;这道题目有点类似之前Merge Intervals系列题目，都是区间重叠的问题，但是这道题目的难度提升了不少，原因是还要考虑高度。题目给出了区间以及每个区间的高度，要求我们求出所有的**key point**，对**key point**的定义是所有横线的左端点（感觉中文直译过来不是很准确，还是按照题目图片理解最好，就是天际线的意思）。我们先看key point的横纵坐标有什么特点，他们的横坐标一定是其中一个区间的横坐标，纵坐标也肯定是其中一个区间的纵坐标，不存在两个区间直接需要计算的东西。

&emsp;&emsp;然后我们按着题目给出的例子，分析一下有哪些情况。第一种，蓝色区间和紫色区间的左边，以及绿色区间和黄色区间的右边，这些都是单个区间的边界决定的，比较直接；第二种，红色区间的左端点，虽然红色区间被蓝色区间包含，但是因为红色区间较高，所以它的左端点也是key point；第三种，红色区间和绿色区间重叠导致的key point，这种情况是最复杂的，因为它的横坐标是红色区间的右端点，纵坐标是绿色区间的高度。

&emsp;&emsp;我们来重点分析第三种情况。首先这个情况出现的原因是绿色区间和红色区间重叠了，而且绿色区间的高度比较低，所以就会有了这个key point。所以这里的高度信息可能是需要记录的，同时我们还要记录下区间的左右坐标，因为只有左坐标才要考虑，右坐标不需要。所以第三种情况出现的条件是**有一个更矮的区间叠进了一个更高的区间**。

&emsp;&emsp;然后就来看解题的思路了：首先对所有区间排序，然后从左到右进行处理。记录下当前没有遍历完成的区间的最大高度，如果当前遍历到区间的左端：

1. 当前区间的高度小于最大高度，则以当前区间左端的横坐标作为横坐标，以最大高度作为纵坐标构成key point；
2. 当前区间的高度就是最大高度，只接用当前区间左端的横坐标作为横坐标，以当前区间的高度作为纵坐标构成key point；

------

## Solution

&emsp;&emsp;为了区分区间的左右端点，这里用了一个coding的小技巧，使用pair的形式记录，是为了便于排序。左端点的值采用负数，右端点的值采用正数，这样能够保证在排序的时候左端点一定出现在右端点的前面。因为要判断一个区间是否遍历完，所以用multiset记录下高度，同时也能利用set的有序性找到最大高度。

&emsp;&emsp;特别注意，因为最后一个点的高度一定是0，所以multiset初始状态需要插入一个0。

------

## Code

```c++
class Solution {
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
        vector<pair<int, int>> temp;
        vector<vector<int>> result;
        multiset<int> s;
        int size = buildings.size();
        
        for (int i = 0; i < size; i++) {
            temp.push_back(make_pair(buildings[i][0], -buildings[i][2]));
            temp.push_back(make_pair(buildings[i][1], buildings[i][2]));
        }
        sort(temp.begin(), temp.end());
        s.insert(0); // default
        
        int current = 0, pre = 0;
        for (auto point: temp) {
            if (point.second < 0) {
                // left side
                s.insert(-point.second);
            } else {
                // right side
                s.erase(s.find(point.second));
            }
            
            current = *s.rbegin(); // largest
            if (current != pre) {
                // key point
                result.push_back({point.first, current});
                pre = current;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是区间问题的立体版本，要多考虑一个高度的因素，但是本质还是抓住区间的特点。使用multiset去记录区间的点是比较tricky的做法，一下子可能比较难想到，所以这里就做一下总结。这道题目的分享到这里，谢谢您的支持！
