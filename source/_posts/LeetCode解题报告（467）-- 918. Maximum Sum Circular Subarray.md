---
title: LeetCode解题报告（467）-- 918. Maximum Sum Circular Subarray
mathjax: true
date: 2021-12-27 23:25:17
---

## Problem

Given a **circular integer array** `nums` of length `n`, return *the maximum possible sum of a non-empty **subarray** of* `nums`.

A **circular array** means the end of the array connects to the beginning of the array. Formally, the next element of `nums[i]` is `nums[(i + 1) % n]` and the previous element of `nums[i]` is `nums[(i - 1 + n) % n]`.

A **subarray** may only include each element of the fixed buffer `nums` at most once. Formally, for a subarray `nums[i], nums[i + 1], ..., nums[j]`, there does not exist `i <= k1`, `k2 <= j` with `k1 % n == k2 % n`.

<!-- more -->

**Example 1:**

```
Input: nums = [1,-2,3,-2]
Output: 3
Explanation: Subarray [3] has maximum sum 3.
```

**Example 2:**

```
Input: nums = [5,-3,5]
Output: 10
Explanation: Subarray [5,5] has maximum sum 5 + 5 = 10.
```

**Example 3:**

```
Input: nums = [-3,-2,-3]
Output: -2
Explanation: Subarray [-2] has maximum sum -2.
```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 3 * 104`
- `-3 * 104 <= nums[i] <= 3 * 104`

---

## Analysis

&emsp;&emsp;这道题目是最大子数组的变种，这里的背景是数组变成循环数组了，但是我们仍然可以参考之前的做法。答案有两种情况，第一种是和最大子数组一样，刚好在中间部分，这部分的逻辑可以照搬；第二种是最大的子数组包括两部分，开头的一部分加上结尾的一部分。

&emsp;&emsp;我们重点来关注第二种情况，整个数组如果开头的部分加上结尾的部分是最大的话，那么就说明剩下的部分就是最小的，至此我们再一次把问题转化成最小子数组，这个也不难吧。

&emsp;&emsp;最后我们只需要把两种结果对比一下，取较大的就可以了。这里还有一个edge case要考虑，当整个数组都是负数时，，第二种情况求最小子数组的和将会是整个数组的和，那么剩下部分就是0，但是答案应该是最大的那个负数，所以这里要多加一个判断，当`sum - minSum == 0`时，说明直接取`maxSum`。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        int minSum = INT_MAX, maxSum = INT_MIN, curMin = 0, curMax = 0, sum = 0;
        for (int &num: nums) {
            curMax = max(curMax + num, num);
            maxSum = max(maxSum, curMax);
            
            curMin = min(curMin + num, num);
            minSum = min(minSum, curMin);
            
            sum += num;
        }
        
        return (sum - minSum == 0) ? maxSum: max(maxSum, sum - minSum);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是最大子数组的变种题，虽然变成了循环数组，但是把它分开两种情况看，又回到了最大子数组的题目。这道题目的分享到这里，感谢你的支持！
