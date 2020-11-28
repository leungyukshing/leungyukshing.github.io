---
title: LeetCode解题报告（230）-- 239. Sliding Window Maximum
tags:
  - LeetCode
mathjax: true
date: 2020-11-28 20:03:45

---

## Problem

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return *the max sliding window*.

<!-- more -->

**Example 1:**

```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]
```

**Example 3:**

```
Input: nums = [1,-1], k = 1
Output: [1,-1]
```

**Example 4:**

```
Input: nums = [9,11], k = 2
Output: [11]
```

**Example 5:**

```
Input: nums = [4,-2], k = 2
Output: [4]
```

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `1 <= k <= nums.length`

------

## Analysis

&emsp;&emsp;这道题目是滑动窗口类的题目，题目给定了窗口的大小，要求找出每个窗口下的最大值。这道题目的难点也就是在于动态维护这个最大值，我们不能只用一个值去记录，因为一旦窗口划过之后，我们就要找到第二大的作为最大值。想到这里，就应该借助一些特殊的数据结构来完成。支持增删而且同时还维护最大值的数据结构就是**最大堆**。遍历数组，首先排除掉不在窗口范围内的最大值，然后把当前的数字放入堆中，最后把当前的最大追加入到答案中即可。

------

## Solution

&emsp;&emsp;C++中的最大堆可以使用STL提供的优先队列，其底层实现也是基于堆的，队头就是最大值。

------

## Code

```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        priority_queue<pair<int, int>> q;
        vector<int> result;
        
        int size = nums.size();
        for (int i = 0; i < size; i++) {
            while (!q.empty() && q.top().second <= i - k) {
                q.pop();
            }
            q.push({nums[i], i});
            if (i >= k - 1) {
                result.push_back(q.top().first);
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是一道很好的数据结构应用题，动态维护最大（最小）值的时候我们优先考虑堆这个数据结构。这道题目的分享到这里，谢谢您的支持！
