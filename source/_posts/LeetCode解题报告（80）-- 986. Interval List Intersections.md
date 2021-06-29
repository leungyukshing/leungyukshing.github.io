---
title: LeetCode解题报告（80）-- 986. Interval List Intersections
tags:
  - LeetCode
mathjax: true
abbrlink: 48295
date: 2019-12-24 17:28:45
---

## Problem

Given two lists of **closed** intervals, each list of intervals is pairwise disjoint and in sorted order.

Return the intersection of these two interval lists.

*(Formally, a closed interval [a, b] (with a <= b) denotes the set of real numbers x with a <= x <= b.  The intersection of two closed intervals is a set of real numbers that is either empty, or can be represented as a closed interval.  For example, the intersection of [1, 3] and [2, 4] is [2, 3].)*

<!-- more -->

**Example 1:**

![Pic](https://assets.leetcode.com/uploads/2019/01/30/interval1.png)

```
Input: A = [[0,2],[5,10],[13,23],[24,25]], B = [[1,5],[8,12],[15,24],[25,26]]
Output: [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
Reminder: The inputs and the desired output are lists of Interval objects, and not arrays or lists.
```

**Note:**

1. `0 <= A.length < 1000`
2. `0 <= B.length < 1000`
3. `0 <= A[i].start, A[i].end, B[i].start, B[i].end < 10^9`

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

------

## Analysis

&emsp;&emsp;这是一道区间交集的题目，题目意思比较简单，给出A、B两个区间数组，求出他们里面区间的交集（包括长度为0的交集）。一开始想可能会比较乱，我们不妨把这个问题分成三个子问题来思考：

- 如何判断相交？
- 相交后怎么取重合的区间？
- 判断完一组后，是找A的下一个还是找B的下一个？

&emsp;&emsp;对于第一个问题，相交的判断条件很多，我们可以从相反的角度来看，排除掉不相交的，剩下的就是相交的。不相交的情况如下：

1. `A.end < B.begin`
2. `B.end < A.begin`

&emsp;&emsp;第二个问题，重合区间的取法非常简单，原则就是**区间尽可能小**，所以起始点选大的，终止点选小的。

- `begin`:`max(A.begin, B.begin)`
- `end`:`min(A.end, B.end)`

&emsp;&emsp;第三个问题，如何移动到下一个？因为找的是重合区间，所以移动的当然是终止点小的那个啦。

&emsp;&emsp;分别分析了这三个问题后，我们拼在一起就是我们解题的思路了。`A`和`B`两个数组同时开始，判断是否有重合，若无，则移动到下一个；若有，则提取出重合区间，然后移动到下一个。

------

## Solution

&emsp;&emsp;需要注意两个区间有一个点重合也算重合，判断条件需要注意是否有`==`。

------

## Code

```c++
class Solution {
public:
    vector<vector<int>> intervalIntersection(vector<vector<int>>& A, vector<vector<int>>& B) {
        vector<vector<int>> result;
        int size1 = A.size();
        int size2 = B.size();
        int i = 0, j = 0;
        
        while (i < size1 && j < size2) {
            if (A[i][1] < B[j][0]) {
                i++;
            }
            else if (B[j][1] < A[i][0]) {
                j++;
            }
            else {
                vector<int> temp;
                temp.push_back(max(A[i][0], B[j][0]));
                temp.push_back(min(A[i][1], B[j][1]));
                result.push_back(temp);
                if (A[i][1] < B[j][1]) {
                    i++;
                }
                else {
                    j++;
                }
            }
        }
        return result;
    }
};
```

------

## Summary

 &emsp;&emsp;这道重叠区间的题目算法上没有很难的地方，主要是分析的过程需要拆解不同的步骤。区间问题的分析抓住起点和终点基本上都能够很容易解决。这道题目就和大家分享到这里，欢迎大家评论、转发，谢谢您的支持！
