---
title: LeetCode解题报告（83）-- 1029. Two City Scheduling
tags:
  - LeetCode
mathjax: true
abbrlink: 63711
date: 2020-06-08 12:08:11
---

## Problem

There are `2N` people a company is planning to interview. The cost of flying the `i`-th person to city `A` is `costs[i][0]`, and the cost of flying the `i`-th person to city `B` is `costs[i][1]`.

Return the minimum cost to fly every person to a city such that exactly `N` people arrive in each city.

<!-- more -->

**Example:**

```
Input: [[10,20],[30,200],[400,50],[30,20]]
Output: 110
Explanation: 
The first person goes to city A for a cost of 10.
The second person goes to city A for a cost of 30.
The third person goes to city B for a cost of 50.
The fourth person goes to city B for a cost of 20.

The total minimum cost is 10 + 30 + 50 + 20 = 110 to have half the people interviewing in each city.
```

**Note:**

1. `1 <= costs.length <= 100`
2. It is guaranteed that `costs.length` is even.
3. `1 <= costs[i][0], costs[i][1] <= 1000`

------

## Analysis

&emsp;&emsp;先来看题目的意思，题目给出一个二维矩阵，每一行都有两个值，分别是去A城市和B城市的花费。然后我们需要选出一半人去A城市，另一半人去B城市，最后使得花费的总和最小。

&emsp;&emsp;首先穷举肯定是不可能的，计算量太大。我们不妨换一种思路，先计算去A城市和B城市之间花费的差额`costs[i][0] - costs[i][1]`。然后对这个差额进行排序（从小到大），那么这个差额越小，说明去B城市比去A城市的花费更少。题目要求选一半人，我们根据这个差额，选取最小的一半就可以了。

------

## Solution

&emsp;&emsp;首先遍历一遍就算出差额`diff`，并计算去所有人都去A城市的总花费。然后对`diff`进行从小到大排序。选取较小的一半求和，并加到总花费中，得出来的结果就是答案了。

------

## Code

```c++
class Solution {
public:
    int twoCitySchedCost(vector<vector<int>>& costs) {
        vector<int> diff;
        int size = costs.size();
        int total = 0;
        
        for (int i = 0; i < size; i++) {
            total += costs[i][0];
            diff.push_back(costs[i][1] - costs[i][0]);
        }
        sort(diff.begin(), diff.end());
        
        for (int i = 0; i < size / 2; i++) {
            total += diff[i];
        }
        
        return total;
    }
};
```

------

## Summary

 &emsp;&emsp;由于题目有一半人的限制，所以并不需要用到动态规划来做。这里只需要用简单的排序就能做出来。希望这篇博客能够帮助到大家，谢谢！
