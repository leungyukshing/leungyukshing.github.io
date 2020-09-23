---
title: LeetCode解题报告（168）-- 134. Gas Station
tags:
  - LeetCode
mathjax: true
date: 2020-09-23 15:49:11

---

## Problem

There are *N* gas stations along a circular route, where the amount of gas at station *i* is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from station *i* to its next station (*i*+1). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1.

**Note:**

- If there exists a solution, it is guaranteed to be unique.
- Both input arrays are non-empty and have the same length.
- Each element in the input arrays is a non-negative integer.

<!-- more -->

**Example 1:**

```
Input: 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]

Output: 3

Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```

**Example 2:**

```
Input: 
gas  = [2,3,4]
cost = [3,4,3]

Output: -1

Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
```

------

## Analysis

&emsp;&emsp;这是一道加油问题，因为题目要求的是走完一圈，所以这里判断能不能走完一圈很重要的条件就是总的油量和总的消耗。如果总的油量大于总的消耗，那么是肯定可以走完的。上面这个比较好计算，只需要累加起来最后判断正负就可以了。

&emsp;&emsp;然后要判断的是起点，我们没有必要所有的点都整个循环遍历一次去验证，实际上我们只需要往后遍历就可以了。同样地我们也记录从某个点开始的油量与消耗的差，如果这个差小于0则说明这个点不能作为起点，于是就选择后面一个点作为起点。因为我们有总的油量和消耗的记录做保证，所以遍历到数组结束就可以了。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int total = 0, sum = 0, result = 0, size = gas.size();
        for (int i = 0; i < size; i++) {
            total += gas[i] - cost[i];
            sum += gas[i] - cost[i];
            if (sum < 0) {
                result = i + 1;
                sum = 0;
            }
        }
        if (total < 0) {
            return -1;
        } else {
            return result;
        }
    }
};
```

------

## Summary

&emsp;&emsp;加油问题重点还是把握住油量和油耗的关系，这道题目中，只有油量减去油耗之后还为正的情况下才是有效的前进条件，把握住这个就能很容易地计算出起点了。这道题的分析到这里，谢谢！

