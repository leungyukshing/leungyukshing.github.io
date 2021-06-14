---
title: LeetCode解题报告（394) -- 128. Longest Consecutive Sequence
tags:
  - LeetCode
mathjax: true
date: 2021-06-14 15:21:47

---

## Problem

Given an unsorted array of integers `nums`, return *the length of the longest consecutive elements sequence.*

You must write an algorithm that runs in `O(n)` time.

<!-- more -->

**Example 1:**

```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

**Example 2:**

```
Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9

```



**Constraints:**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

------

## Analysis

&emsp;&emsp;题目的通过两个数组，给出了每个工人的速度和效率，定义整个团队的performance是最小的效率乘以速度之和。要求我们从中选出`k`个成员，找到能够最大化的performance。首先，这里的关键应该是最小的效率，因为每个人的速度都要乘以它，所以我们优先考虑效率。我们先根据效率从大到小排序，然后把每一个成员的速度加起来，全局维护一个极大值。

&emsp;&emsp;然后，问题就转化为如果人数超过了`k`个人怎么办？因为效率是从大到小遍历的，所以当处理到第`i`个工人时，肯定是以第`i`个工人的效率为最低效率，关键是速度。因为我们要的是速度之和最大，很自然地我们就需要剔除掉速度最小的一个工人了，所以我们需要用一个最小堆去记录。当超过`k`个人之后，剔除掉速度最小的即可。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    struct Engineer {
        int speed, efficiency;
        bool operator < (const Engineer& rhs) const {
            return speed > rhs.speed;
        }
    };
    
    int maxPerformance(int n, vector<int>& speed, vector<int>& efficiency, int k) {
        vector<Engineer> v;
        priority_queue<Engineer> q;
        for (int i = 0; i < n; i++) {
            v.push_back({speed[i], efficiency[i]});
        }
        
        sort(v.begin(), v.end(), [] (const Engineer &u, const Engineer &v) {
            return u.efficiency > v.efficiency;
        });
        
        long long result = 0, sum = 0;
        for (int i = 0; i < n; i++) {
            long long minEfficiency = v[i].efficiency;
            long long sumSpeed = sum + v[i].speed;
            result = max(result, sumSpeed * minEfficiency);
            q.push(v[i]);
            sum += v[i].speed;
            
            if (q.size() == k) {
                sum -= q.top().speed;
                q.pop();
            }
        }
        int MOD = 1e9 + 7;
        return result % MOD;
    }
};
```

------

## Summary

&emsp;&emsp;这道题难度中等，面对这种有数量限制的题目，一般采用的是滑动窗口（连续）或者堆（非连续）。这道题目的分享到这里，感谢你的支持！
