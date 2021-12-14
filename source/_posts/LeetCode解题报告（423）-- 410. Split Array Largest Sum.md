---
title: LeetCode解题报告（423）-- 410. Split Array Largest Sum
mathjax: true
date: 2021-12-12 16:32:41
---

## Problem

Given an array `nums` which consists of non-negative integers and an integer `m`, you can split the array into `m` non-empty continuous subarrays.

Write an algorithm to minimize the largest sum among these `m` subarrays.

<!-- more -->

**Example 1:**

```
Input: nums = [7,2,5,10,8], m = 2
Output: 18
Explanation:
There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8],
where the largest sum among the two subarrays is only 18.
```

**Example 2:**

```
Input: nums = [1,2,3,4,5], m = 2
Output: 9
```

**Example 3:**

```
Input: nums = [1,4,4], m = 3
Output: 4
```

**Constraints:**

- `1 <= nums.length <= 1000`
- `0 <= nums[i] <= 106`
- `1 <= m <= min(50, nums.length)`

------

## Analysis

&emsp;&emsp;题目给出了一个数组`nums`以及一个`m`，要求把数组拆分成`m`个子数组，计算这`m`个子数组中最大的和，然后要找到一种拆分的方式使得这个和最小。对于数组拆分类的题目，因为所有元素都要使用到，所以一般会考虑dp。一般数组拆分类的dp定义都会是一个二维数组，比如说`dp[i][j]`为把前`j`个数字拆成`i`组所能得到的最小的各个子数组和的最大值。遍历两个状态，`i`的范围是`[1, m]`，`j`的范围是`[1, n]`，在处理`j`的时候，实际上需要遍历`[1, j]`这个区间，以`k`为分割点，前`k`个数字的结果就是`dp[i - 1][k]`（已知），后面数字的和作为一组。这里因为需要求后面数字的和，所以要首先计算前缀和。最后用`min(dp[i - 1][k], presum[j] - presum[k])`去更新`dp[i][j]`即可。答案就是`dp[m][n]`。

&emsp;&emsp;这道题目除了dp，还有另一种方法可以做。我们重新看题，一是数组中所有的元素都要被使用到，二是有一个限制条件，三是要求的答案是一个子数组的和，是有规定区间的。这三个特点就指向了二分答案的思路。我们先来看答案的区间，两种极端case：

1. 如果`m = 1`，说明整个数组就是答案，答案就是数组的元素和；
2. 如果`m = n`，说明每个数字单独是一个子数组，答案就是最大的那个数字。

&emsp;&emsp;上面说的就是答案的区间，得到区间之后我们就对答案进行二分搜索。接下来我们来看怎么二分，对于每一个`mid`，我们找出小于等于这个`mid`的区间数量`x`，如果`x < m`，说明`mid`太大了，因此要向左半部分搜索；反之，要向右半部分搜索。特殊情况是`x == m`，这里因为我们要的是最小的，所以应该是左边界，因此和`x < m`的处理情况一样。

## Solution

&emsp;&emsp;无

------

## Code

### Method1: Dynamic Programming

```c++
class Solution {
public:
    int splitArray(vector<int>& nums, int m) {
        int n = nums.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, INT_MAX));
        vector<int> presum(n + 1, 0);
        
        for (int i = 1; i <= n; i++) {
            presum[i] = presum[i - 1] + nums[i - 1];
        }
        
        dp[0][0] = 0;
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                for (int k = i - 1; k < j; ++k) {
                    dp[i][j] = min(dp[i][j], max(dp[i - 1][k], presum[j] - presum[k]));
                }
            }
        }
        return dp[m][n];
    }
};
```



### Method 2: Binary Search

```c++
class Solution {
public:
    int splitArray(vector<int>& nums, int m) {
        long left = 0, right = 0;
        for (int num: nums) {
            left = max(left, (long)num);
            right += num;
        }
        
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (canSplit(nums, m, mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
private:
    bool canSplit(vector<int>& nums, int m, int mid) {
        long sum = 0, count = 1;
        for (int num: nums) {
            sum += num;
            if (sum > mid) {
                sum = num;
                count++;
                if (count > m) {
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

&emsp;&emsp;这道题目属于难度比较大的数组类题目，dp的思路比较明确，但是遍历的范围难确定。二分的思路倒是很直观，但是比较难想到。这道题目的分享到这里，感谢你的支持！
