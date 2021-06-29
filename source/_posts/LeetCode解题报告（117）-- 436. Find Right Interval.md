---
title: LeetCode解题报告（117）-- 436. Find Right Interval
tags:
  - LeetCode
mathjax: true
abbrlink: 26246
date: 2020-08-27 16:13:58
---

## Problem

Given a set of intervals, for each of the interval i, check if there exists an interval j whose start point is bigger than or equal to the end point of the interval i, which can be called that j is on the "right" of i.

For any interval i, you need to store the minimum interval j's index, which means that the interval j has the minimum start point to build the "right" relationship for interval i. If the interval j doesn't exist, store -1 for the interval i. Finally, you need output the stored value of each interval as an array.

**Note:**

1. You may assume the interval's end point is always bigger than its start point.
2. You may assume none of these intervals have the same start point.

<!-- more -->

**Example 1:**

```
Input: [ [1,2] ]

Output: [-1]

Explanation: There is only one interval in the collection, so it outputs -1.
```

**Example 2:**

```
Input: [ [3,4], [2,3], [1,2] ]

Output: [-1, 0, 1]

Explanation: There is no satisfied "right" interval for [3,4].
For [2,3], the interval [3,4] has minimum-"right" start point;
For [1,2], the interval [2,3] has minimum-"right" start point.
```

**Example 3:**

```
Input: [ [1,4], [2,3], [3,4] ]

Output: [-1, 2, -1]

Explanation: There is no satisfied "right" interval for [1,4] and [3,4].
For [2,3], the interval [3,4] has minimum-"right" start point.
```

**NOTE:** input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

------

## Analysis

&emsp;&emsp;题目给出了一系列的区间，要求找到每个区间的右区间。判断右区间是比较容易的，对于A、B两个区间，如果B是A的右区间，那么有`A.end < B.start`。

&emsp;&emsp;因为答案需要返回的是右区间的下标，所以不可能直接打乱顺序进行排序。这个时候就需要用一个hashmap来记录顺序了。对于某一个区间，找到其他的右区间只需要比对其他区间的start，所以我们干脆就把所有区间的start提取出来放到一个数组，map记录每个start对应的下标。然后对这些start排序，从大到小。

&emsp;&emsp;然后遍历每一个区间，在start数组中找到第一个比end要小的，假设是第`j` 个。那么第`j-1`个就是比end要大的，而且是最贴近的右区间，通过反查map就能知道下标了。

------

## Solution

&emsp;&emsp;因为要找到最靠近的右区间，所以要先找到第一个比end小的，再往前一个。如果是第一个的话，就说明没有右区间，这个边界条件也需要考虑。

------

## Code

```c++
class Solution {
public:
    vector<int> findRightInterval(vector<vector<int>>& intervals) {
        map<int, int> m;
        vector<int> allStarts;
        vector<int> result;
        int size = intervals.size();
        
        for (int i = 0; i < size; i++) {
            allStarts.push_back(intervals[i][0]);
            m.insert(make_pair(intervals[i][0], i));
        }
        
        sort(allStarts.rbegin(), allStarts.rend());
        
        for (int i = 0; i < size; i++) {
            int end = intervals[i][1];
            int j = 0;
            for (j = 0; j < size; j++) {
                if (end > allStarts[j]) {
                    break;
                }
            }
            if (j == 0) {
                result.push_back(-1);
            } else {
                result.push_back(m[allStarts[j - 1]]);
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;区间的题目之前碰到过不少，这道题也是综合了几个知识点，一个是区间类题目把握好start和end的比较，第二个是善于使用map来记录对应位置关系。这道题的分析到这里，谢谢！
