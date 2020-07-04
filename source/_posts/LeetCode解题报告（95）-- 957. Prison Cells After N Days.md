---
title: LeetCode解题报告（95）-- 957. Prison Cells After N Days
tags:
  - LeetCode
date: 2020-07-04 14:21:44
mathjax: true

---

## Problem

There are 8 prison cells in a row, and each cell is either occupied or vacant.

Each day, whether the cell is occupied or vacant changes according to the following rules:

- If a cell has two adjacent neighbors that are both occupied or both vacant, then the cell becomes occupied.
- Otherwise, it becomes vacant.

(Note that because the prison is a row, the first and the last cells in the row can't have two adjacent neighbors.)

We describe the current state of the prison in the following way: `cells[i] == 1` if the `i`-th cell is occupied, else `cells[i] == 0`.

Given the initial state of the prison, return the state of the prison after `N` days (and `N` such changes described above.)

<!-- more -->

**Example 1:**

```
Input: cells = [0,1,0,1,1,0,0,1], N = 7
Output: [0,0,1,1,0,0,0,0]
Explanation: 
The following table summarizes the state of the prison on each day:
Day 0: [0, 1, 0, 1, 1, 0, 0, 1]
Day 1: [0, 1, 1, 0, 0, 0, 0, 0]
Day 2: [0, 0, 0, 0, 1, 1, 1, 0]
Day 3: [0, 1, 1, 0, 0, 1, 0, 0]
Day 4: [0, 0, 0, 0, 0, 1, 0, 0]
Day 5: [0, 1, 1, 1, 0, 1, 0, 0]
Day 6: [0, 0, 1, 0, 1, 1, 0, 0]
Day 7: [0, 0, 1, 1, 0, 0, 0, 0]
```

**Example 2:**

```
Input: cells = [1,0,0,1,0,0,1,0], N = 1000000000
Output: [0,0,1,1,1,1,1,0]
```

**Note:**

1. `cells.length == 8`
2. `cells[i]` is in `{0, 1}`
3. `1 <= N <= 10^9`

------

## Analysis

&emsp;&emsp;这道题目是一道比较新的状态转移题目，题目给出了新旧状态之间的转移规则，然后给出了一个转移的次数`N`。

&emsp;&emsp;先来看看转移规则：

1. 如果左右两边相等，则置为1；
2. 最左边和最右边都设置为0

&emsp;&emsp;规则并不复杂，用两个vector就能记录新旧状态，然后不断更新状态就可以了。我们再来看看这个转移的次数，这个次数才是解题的关键，测试用例中的N有可能非常大，如果我们还是做N次转移的话，计算量会非常大。这道题的巧妙之处也在这里，网上有大神找到这样一个规律，状态的更新是有周期性的，以14为一个周期，所以我们只需要对N取余就能够知道实际上要做多少次状态转移了。

------

## Solution

&emsp;&emsp;需要注意如果N刚好是14的倍数，则需要做14次状态转移。

------

## Code

```c++
class Solution {
public:
    vector<int> prisonAfterNDays(vector<int>& cells, int N) {
        N %= 14;
        if (N == 0) {
            N = 14;
        }
        int size = cells.size();
        vector<int> result = cells;
        for (int i = 0; i < N; i++) {
            vector<int> newCells(size, 0);
            newCells[0] = 0;
            newCells[size - 1] = 0;
            for (int j = 1; j < size - 1; j++) {
                if (result[j - 1] == result[j + 1]) {
                    newCells[j] = 1;
                } else {
                    newCells[j] = 0;
                }
            }
            result = newCells;
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题还是比较新颖的，对于这类状态类的题目，我们可以先通过在纸上推理，找到规律，这样对解题有很大的帮助。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
