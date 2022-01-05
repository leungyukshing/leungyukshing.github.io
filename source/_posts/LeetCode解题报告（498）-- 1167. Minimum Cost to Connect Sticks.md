---
title: LeetCode解题报告（498）-- 1167. Minimum Cost to Connect Sticks
mathjax: true
date: 2022-01-04 23:08:27
---

## Problem

You have some number of sticks with positive integer lengths. These lengths are given as an array `sticks`, where `sticks[i]` is the length of the `ith` stick.

You can connect any two sticks of lengths `x` and `y` into one stick by paying a cost of `x + y`. You must connect all the sticks until there is only one stick remaining.

Return *the minimum cost of connecting all the given sticks into one stick in this way*.

<!-- more -->

**Example 1:**

```
Input: sticks = [2,4,3]
Output: 14
Explanation: You start with sticks = [2,4,3].
1. Combine sticks 2 and 3 for a cost of 2 + 3 = 5. Now you have sticks = [5,4].
2. Combine sticks 5 and 4 for a cost of 5 + 4 = 9. Now you have sticks = [9].
There is only one stick left, so you are done. The total cost is 5 + 9 = 14.
```

**Example 2:**

```
Input: sticks = [1,8,3,5]
Output: 30
Explanation: You start with sticks = [1,8,3,5].
1. Combine sticks 1 and 3 for a cost of 1 + 3 = 4. Now you have sticks = [4,8,5].
2. Combine sticks 4 and 5 for a cost of 4 + 5 = 9. Now you have sticks = [9,8].
3. Combine sticks 9 and 8 for a cost of 9 + 8 = 17. Now you have sticks = [17].
There is only one stick left, so you are done. The total cost is 4 + 9 + 17 = 30.
```

**Example 3:**

```
Input: sticks = [5]
Output: 0
Explanation: There is only one stick, so you don't need to do anything. The total cost is 0.
```



**Constraints:**

- `1 <= sticks.length <= 104`
- `1 <= sticks[i] <= 104`

---

## Analysis

&emsp;&emsp;这道题目是连接火柴问题，题目给出了一系列火柴，定义每次连接的cost是两条火柴长度的和，问把所有火柴连成1根最少的花费是多少？首先我们这道题目的cost最大和最小是怎么来的，因为每根火柴在计算的时候对最后的cost贡献是不一样的，比如example1中，如果我们先连2和4，然后连6和3，那么2贡献2次，4贡献2次，3贡献1次，总cost是15，并不是最少的。这里就能看出问题了，越先连接的火柴，它的长度对cost的贡献就越多，所以这里就是greedy的思想了，每次我都选最短的两根火柴连接，这样总的cost就会最少。

&emsp;&emsp;之前的blog也说了很多次了，greedy的题目一般配合priority queue使用，所以这里直接把所有的火柴都放到priority queue中（最小堆），每次取顶部两根，然后求和后放回去，直至队列中只有一根火柴，那个就是答案。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int connectSticks(vector<int>& sticks) {
        priority_queue<int, vector<int>, greater<int>> pq;
        for (int &num: sticks) {
            pq.push(num);
        }
        
        int result = 0;
        while (pq.size() > 1) {
            int s1 = pq.top();
            pq.pop();
            int s2 = pq.top();
            pq.pop();
            result += s1 + s2;
            pq.push(s1 + s2);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难点在于如何分析出需要使用greedy算法，突破口在于分析清楚最小值的情况是怎么来的，这样才能知道是因为不同的长度对最终cost贡献的次数不同。至于后面使用priority queue就比较简单。这道题目的分享到这里，感谢你的支持！

