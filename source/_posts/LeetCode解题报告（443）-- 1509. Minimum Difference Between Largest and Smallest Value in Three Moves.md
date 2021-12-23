---
title: LeetCode解题报告（443）-- 1509. Minimum Difference Between Largest and Smallest Value in Three Moves
mathjax: true
date: 2021-12-23 16:33:46
---

## Problem

You are given an integer array `nums`. In one move, you can choose one element of `nums` and change it by **any value**.

Return *the minimum difference between the largest and smallest value of `nums` after performing **at most three moves***.

<!-- more -->

**Example 1:**

```
Input: nums = [5,3,2,4]
Output: 0
Explanation: Change the array [5,3,2,4] to [2,2,2,2].
The difference between the maximum and minimum is 2-2 = 0.
```

**Example 2:**

```
Input: nums = [5,3,2,4]
Output: 0
Explanation: Change the array [5,3,2,4] to [2,2,2,2].
The difference between the maximum and minimum is 2-2 = 0.
```

**Constraints:**

- `1 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

------

## Analysis

&emsp;&emsp;题目给出一个数组`nums`，允许我们最多修改三个位置的值，问修改过后，最大值和最小值的差要最小，要求返回这个差。一般处理这种最大最小的问题，可以先排序，好处是小的就在左边，大的在右边，处理起来方便。排序后其实就非常简单了，一共能修改三个元素，其实我们并不关心修改后是什么值，我们只关心修改完之后最大的和最小的值是什么。所以就可以直接列举所有的情况：

1. 前面改0个，后面改3个，最小值就是`nums[0]`，最大值就是`nums[n - 4]`；
2. 前面改1个，后面改2个，最小值就是`nums[1]`，最大值就是`nums[n - 3]`；
3. 前面改2个，后面改1个，最小值就是`nums[2]`，最大值就是`nums[n - 2]`；
4. 前面改1个，后面改0个，最小值就是`nums[3]`，最大值就是`nums[n - 1]`。

&emsp;&emsp;根据上面还能得出边界条件就是当`n < 5`时，可以直接把所有元素都改成一样的，因此结果直接就是0。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int minDifference(vector<int>& nums) {
        int size = nums.size();
        if (size < 5) {
            return 0;
        }
        sort(nums.begin(), nums.end());
        return min({nums[size - 4] - nums[0], nums[size - 3] - nums[1], nums[size - 2] - nums[2], nums[size - 1] - nums[3]});
    }
};
```

------

## Summary

&emsp;&emsp;这道题目更像是一道智力题，只需要一个排序就能把思路理清楚，然后列举出所有的情况。这道题目的分享到这里，感谢你的支持！
