---
title: LeetCode解题报告（266）-- 881. Boats to Save People
tags:
  - LeetCode
mathjax: true
date: 2021-01-15 17:40:28

---

## Problem

The `i`-th person has weight `people[i]`, and each boat can carry a maximum weight of `limit`.

Each boat carries at most 2 people at the same time, provided the sum of the weight of those people is at most `limit`.

Return the minimum number of boats to carry every given person.  (It is guaranteed each person can be carried by a boat.)

<!-- more -->

**Example 1:**

```
Input: people = [1,2], limit = 3
Output: 1
Explanation: 1 boat (1, 2)
```

**Example 2:**

```
Input: people = [3,2,2,1], limit = 3
Output: 3
Explanation: 3 boats (1, 2), (2) and (3)
```

**Example 3:**

```
Input: people = [3,5,3,4], limit = 5
Output: 4
Explanation: 4 boats (3), (3), (4), (5)
```

**Note**:

- `1 <= people.length <= 50000`
- `1 <= people[i] <= limit <= 30000`

------

## Analysis

&emsp;&emsp;这道题目看上去策略类题目，题目给出一个数组，代表人的体重，规定了每条船的限重，并且最多只能载两个人，问载完所有的人需要最少的船的数量。因为这道题目对船的人数做了限制，所以简单了很多。

&emsp;&emsp;从直观上说，我们期望的是一个比较重的人和一个比较轻的人坐一条船，所以我们先对数组进行排序。因为每次都需要把重的人先考虑，然后再看看能不能带上一个比较轻的人，。每处理一次就需要一艘船，用一个变量累加即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int numRescueBoats(vector<int>& people, int limit) {
        int size = people.size();
        sort(people.begin(), people.end());
        int left = 0, right = size - 1;
        int result = 0;
        while (left <= right) {
            if (limit - people[right] - people[left] >= 0) {
                left++;
            }
            right--;
            result++;
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目因为限制了人数为2，所以求解起来非常简单，总体来说还是一个贪心算法。这道题这道题目的分享到这里，谢谢您的支持！
