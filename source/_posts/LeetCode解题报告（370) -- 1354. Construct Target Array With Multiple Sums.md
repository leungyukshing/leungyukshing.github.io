---
title: LeetCode解题报告（370) -- 1354. Construct Target Array With Multiple Sums
tags:
  - LeetCode
mathjax: true
abbrlink: 43075
date: 2021-05-12 19:36:04
---

## Problem

Given an array of integers `target`. From a starting array, `A` consisting of all 1's, you may perform the following procedure :

- let `x` be the sum of all elements currently in your array.
- choose index `i`, such that `0 <= i < target.size` and set the value of `A` at index `i` to `x`.
- You may repeat this procedure as many times as needed.

Return True if it is possible to construct the `target` array from `A` otherwise return False.

<!-- more -->

**Example 1:**

```
Input: target = [9,3,5]
Output: true
Explanation: Start with [1, 1, 1] 
[1, 1, 1], sum = 3 choose index 1
[1, 3, 1], sum = 5 choose index 2
[1, 3, 5], sum = 9 choose index 0
[9, 3, 5] Done
```

**Example 2:**

```
Input: target = [1,1,1,2]
Output: false
Explanation: Impossible to create target array from [1,1,1,1].
```

**Example 3:**

```
Input: target = [8,5]
Output: true
```



**Constraints:**

- `N == target.length`
- `1 <= target.length <= 5 * 10^4`
- `1 <= target[i] <= 10^9`

------

## Analysis

&emsp;&emsp;题目给出一个`target`数组，从一开始有的是全是1的数组，每次操作可以把这个数组的和，替换掉数组中的某个值。问这个`target`数组能否按照这种规则生成出来。我们先看Example1，题目中有解释答案是怎么计算出来的。我们不妨反向看是怎么从结果倒推回去初始状态的。我们先计算出一个`sum = 17`，然后我们找到数组中最大的数，然后用sum减去这个数，同时把这个数移除出数组。移除之后，我们需要把原来的数放回去数组，完成替换。这里特别注意，计算原来的数的时候，不能直接用减法，会TLE，要用取余操作替代。

&emsp;&emsp;中间的转化分析完，然后就要看终止条件。有几个终止条件：

1. 当sum = 1时，说明数组中有两个数，除了最大值之外，剩下的就是1，如`[1, 3]`，这种情况下是一定能成功的；
2. 当最大值为1，说明数组中只有1存在，这是绝大多数变换到最后的情况，所以也是成功的；
3. 当sum为0，说明数组中只有一个元素且不是1，因此转换失败；
4. 当sum比top大，说明需要替换的元素是个负数，不符合题意，转换失败；
5. 当`top % sum == 0`时，说明替换的元素是0，也不符合题意，转换失败。

------

## Solution

&emsp;&emsp;维护数组中最大值很明显就是用大堆了。还需要注意这里`sum`要使用long类型，然后在使用`accumulate`的时候，初始值要用`0LL`，而不是0。

------

## Code

```c++
class Solution {
public:
    bool isPossible(vector<int>& target) {
        priority_queue<int> q(target.begin(), target.end());
        
        long sum = accumulate(target.begin(), target.end(), 0LL);
        
        while (true) {
            int top = q.top();
            q.pop();
            sum -= top;
            if (top == 1 || sum == 1) {
                return true;
            }
            if (sum == 0 || sum > top || top % sum == 0) {
                return false;
            }
            top %= sum;
            sum += top;
            q.push(top);
        }
        return false;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是难度比较大的数组题目，其算法思路较为复杂，同时还需要考虑到多种边界case。这道题目的分享到这里，感谢你的支持！
