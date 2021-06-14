---
title: LeetCode解题报告（397) -- 1696. Jump Game VI
  - LeetCode
mathjax: true
date: 2021-06-14 16:55:45

---

## Problem

You are given a **0-indexed** integer array `nums` and an integer `k`.

You are initially standing at index `0`. In one move, you can jump at most `k` steps forward without going outside the boundaries of the array. That is, you can jump from index `i` to any index in the range `[i + 1, min(n - 1, i + k)]` **inclusive**.

You want to reach the last index of the array (index `n - 1`). Your **score** is the **sum** of all `nums[j]` for each index `j` you visited in the array.

Return *the **maximum score** you can get*.

<!-- more -->

**Example 1:**

```
Input: nums = [1,-1,-2,4,-7,3], k = 2
Output: 7
Explanation: You can choose your jumps forming the subsequence [1,-1,4,3] (underlined above). The sum is 7.
```

**Example 2:**

```
Input: nums = [10,-5,-2,4,0,3], k = 3
Output: 17
Explanation: You can choose your jumps forming the subsequence [10,4,3] (underlined above). The sum is 17.
```

**Example 3:**

```
Input: nums = [1,-5,-20,4,-1,3,-6,-3], k = 2
Output: 0
```



**Constraints:**

- `1 <= nums.length, k <= 105`
- `-104 <= nums[i] <= 104`

------

## Analysis

&emsp;&emsp;题目给出一个数组，从第0个位置开始跳到最后一个，每次可以跳的距离是`[1, k]`，跳的分数是跳过的位置的值之和，问最大值是多少？

&emsp;&emsp;这种问题很明显是往dp方向考虑，定义`dp[i]`为从第`i`个位置开始跳到最后一个位置和的最大值。我们会以值作为优先排序的依据，同时还需要考虑下标，需要判断能否跳到该位置。转移的方程为：`dp[i] = nums[i] + max{dp[i + j]}`，其中`1 <= j <= k`。也就是说，我们从后向前遍历，每次都需要在后面`k`个位置中找出一个最大的dp，加上当前位置的值，作为结果。

&emsp;&emsp;而为了减少每次都需要把后面的`k`个位置都遍历一遍，这里用了堆来记录最大的`k`个数。一旦最大的值所在的下标超出了`k`个位置，则需要移除掉。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
struct Node {
    int idx, val;
    Node(int idx, int val):idx(idx),val(val) {}
    bool operator<(const Node &b) const {
        return val < b.val;
    }
};
    
public:
    int maxResult(vector<int>& nums, int k) {
        priority_queue<Node> q;
        int size = nums.size();
        vector<int> dp(size, 0);
        dp[size - 1] = nums[size - 1];
        q.push({size - 1, nums[size - 1]});
        for (int i = size - 2; i >= 0; i--) {
            Node node = q.top();
            while (node.idx > i + k) {
                q.pop();
                node = q.top();
            }
            // cout << i << " " << node.val << " " << node.idx << endl;
            
            dp[i] = nums[i] + node.val;
            q.push({i, dp[i]});
        }
        return dp[0];
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是难度中等的动态规划题，套路比较经典，是动态规划与堆配合使用。这道题目的分享到这里，感谢你的支持！
