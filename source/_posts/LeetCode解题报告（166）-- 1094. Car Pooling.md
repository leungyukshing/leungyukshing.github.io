---
title: LeetCode解题报告（166）-- 1094. Car Pooling
tags:
  - LeetCode
mathjax: true
abbrlink: 62394
date: 2020-09-21 16:49:30
---

## Problem

You are driving a vehicle that has `capacity` empty seats initially available for passengers.  The vehicle **only** drives east (ie. it **cannot** turn around and drive west.)

Given a list of `trips`, `trip[i] = [num_passengers, start_location, end_location]` contains information about the `i`-th trip: the number of passengers that must be picked up, and the locations to pick them up and drop them off.  The locations are given as the number of kilometers due east from your vehicle's initial location.

Return `true` if and only if it is possible to pick up and drop off all passengers for all the given trips. 

<!-- more -->

**Example 1:**

```
Input: trips = [[2,1,5],[3,3,7]], capacity = 4
Output: false
```

**Example 2:**

```
Input: trips = [[2,1,5],[3,3,7]], capacity = 5
Output: true
```

**Example 3:**

```
Input: trips = [[2,1,5],[3,5,7]], capacity = 3
Output: true
```

**Example 4:**

```
Input: trips = [[3,2,7],[3,7,9],[8,3,9]], capacity = 11
Output: true
```

**Constraints:**

1. `trips.length <= 1000`
2. `trips[i].length == 3`
3. `1 <= trips[i][0] <= 100`
4. `0 <= trips[i][1] < trips[i][2] <= 1000`
5. `1 <= capacity <= 100000`

------

## Analysis

&emsp;&emsp;这道题目是是和区间相关的，场景是上车和下车，不同人都坐一段区间，然后判断是否超出了车的capacity。因为只需要判断，所以我们要把握住车上人数什么时候增加，什么时候减少。在区间开始的时候就增加，下车的时候就减少。因为在车上的人数是通过前面不断累积的，所以我们可以先记住每个节点的变化，上车就是正数，下车就是负数，然后从头开始累加起来就是每个节点车上的人数了。

------

## Solution

&emsp;&emsp;因为题目限制了站的数量是有限的，所以可以直接开一个大小为1000的数组存。

------

## Code

```c++
class Solution {
public:
    bool carPooling(vector<vector<int>>& trips, int capacity) {
        vector<int> temp(1001, 0);
        
        int size = trips.size();
        for (int i = 0; i < size; i++) {
            temp[trips[i][1]] += trips[i][0];
            temp[trips[i][2]] -= trips[i][0];
        }
        
        for (int i = 0; i < 1001; i++) {
            if (i != 0) {
                temp[i] += temp[i - 1];
                if (temp[i] > capacity) {
                    return false;
                }
            }
        }
        return true;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是典型的空间换取时间的解法，利用题目的限制条件去简化解题思路。这道题的分析到这里，谢谢！
