---
title: LeetCode解题报告（438）-- 1986. Minimum Number of Work Sessions to Finish the Tasks
mathjax: true
date: 2021-12-22 22:32:40
---

## Problem

There are `n` tasks assigned to you. The task times are represented as an integer array `tasks` of length `n`, where the `ith` task takes `tasks[i]` hours to finish. A **work session** is when you work for **at most** `sessionTime` consecutive hours and then take a break.

You should finish the given tasks in a way that satisfies the following conditions:

- If you start a task in a work session, you must complete it in the **same** work session.
- You can start a new task **immediately** after finishing the previous one.
- You may complete the tasks in **any order**.

Given `tasks` and `sessionTime`, return *the **minimum** number of **work sessions** needed to finish all the tasks following the conditions above.*

The tests are generated such that `sessionTime` is **greater** than or **equal** to the **maximum** element in `tasks[i]`.

<!-- more -->

**Example 1:**

```
Input: tasks = [1,2,3], sessionTime = 3
Output: 2
Explanation: You can finish the tasks in two work sessions.
- First work session: finish the first and the second tasks in 1 + 2 = 3 hours.
- Second work session: finish the third task in 3 hours.
```

**Example 2:**

```
Input: tasks = [3,1,3,1,1], sessionTime = 8
Output: 2
Explanation: You can finish the tasks in two work sessions.
- First work session: finish all the tasks except the last one in 3 + 1 + 3 + 1 = 8 hours.
- Second work session: finish the last task in 1 hour.
```

**Example 3:**

```
Input: tasks = [1,2,3,4,5], sessionTime = 15
Output: 1
Explanation: You can finish all the tasks in one work session.
```

**Constraints:**

- `n == tasks.length`
- `1 <= n <= 14`
- `1 <= tasks[i] <= 10`
- `max(tasks[i]) <= sessionTime <= 15`

------

## Analysis

&emsp;&emsp;题目给出一个`tasks`数组，以及一个`sessionTime`，每个session里面可以完成多个任务，每个任务只能在一个session内完成，问最少需要几个session。这个看起来是一个贪心问题，但实际上不可行，因为贪心并不能够充分地利用每一个session，所以还是需要把所有的组合都尝试一边。这道题目的突破口其实在Constraint里面，题目规定任务的数量不超过14个，一般来说不超过20都可以用**状态压缩**。

&emsp;&emsp;所谓状态压缩，就是用二进制表示状态，在运算过程中以10进制的形式表示，但归根到底我们只关注二进制的形式。以这道题目为例，假设有3个任务，那么终止状态就是`111`，如果任务1没有完成，那么就是`110`。

&emsp;&emsp;首先我们处理base case，我们有很多状态，每个状态的初始值是多少呢？如果这个状态能够在一个session中完成，那么就初始化为1，其余的就不是base case，在后面状态转移的时候计算。

&emsp;&emsp;状态压缩的状态转移是有套路的，这里给出**枚举二进制子集**的模板：

```c++
// m => total states
for (int i = 1; i < m; ++i) {
    // enumerate the subset of i
    for (int j = i; j > 0; j = (j - 1) & i) {
        // do sth
    }
}
```

&emsp;&emsp;稍微简单解释下，外层循环很容易理解，重点是内层循环怎么把状态`i`的子集枚举完毕。`(j - 1) & i`的作用是不停地去掉最后一个1，例如`i = 10010`，那么第一次的时候`j = (10010 - 1) & 100010 = 10001 & 10010 = 10000`，这样就是把低位的1去掉了。

&emsp;&emsp;回到这个题目看状态转移，状态`j`是状态`i`的一个子集，比如说`i = 1110`，那么`j`就可能是`1100`，此时需要和`0010`一起组成`i`，所以这里用了异或运算取得补集，`dp[i]`只需要维护最小的就可以了。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int minSessions(vector<int>& tasks, int sessionTime) {
        int size = tasks.size();
        int m = 1 << size;
        vector<int> dp(m, 20);
        
        for (int i = 1; i < m; ++i) {
            int spend = 0, state = i, idx = 0;
            while (state) {
                int bit = state & 1;
                if (bit) {
                    spend += tasks[idx];
                }
                state >>= 1;
                idx++;
            }
            if (spend <= sessionTime) {
                dp[i] = 1;
            }
        }
        
        for (int i = 1; i < m; ++i) {
            for (int j = i; j > 0; j = (j - 1) & i) {
                dp[i] = min(dp[i], dp[j] + dp[i ^ j]);
            }
        }
        return dp[m - 1];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是状态压缩dp的典型应用，通过这道题目能够充分理解和使用状压dp，是一道非常好的题目。这道题目的分享到这里，感谢你的支持！
