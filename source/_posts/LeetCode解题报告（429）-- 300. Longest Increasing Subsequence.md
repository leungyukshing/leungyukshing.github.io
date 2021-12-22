---
title: LeetCode解题报告（429）-- 300. Longest Increasing Subsequence
mathjax: true
date: 2021-12-21 22:30:02
---

## Problem

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

<!-- more -->

**Example 1:**

```
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

**Example 2:**

```
Input: nums = [0,1,0,3,2,3]
Output: 4
```

**Example 3:**

```
Input: nums = [7,7,7,7,7,7,7]
Output: 1
```

**Constraints:**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`

**Follow up:** Can you come up with an algorithm that runs in `O(n log(n))` time complexity?

------

## Analysis

&emsp;&emsp;这又是一道极其经典的动态规划题目。题目给出一个数组，里面的数字是乱序的，要求找出最长上升子序列的长度。首先题目只要求返回长度，而不用把整个序列找出来，其次是找子序列，而不是子数组，这两个条件都是很明显的dp提示。

&emsp;&emsp;数组类的dp一般都是一维数组，`dp[i]`表示到数组第`i`个位置最长上升子序列的长度。base case就是每个位置的初始值都是1，因为每个数字本身都是一个上升子序列。接下来就要看状态转移了。子序列的问题要考虑，对于每个位置`i`，上一个状态是什么？是`i - 1`吗？显然不一定是，因为我们要找一个上升子序列，以example2为例，当`i = 4`时，上一个状态应该是`j = 0`、`j = 1`、`j = 2`，因为找的是最长的，只需要把前面所有的情况取一个最大的就可以了。所以这里就有一个$O(n^2)$复杂度。

&emsp;&emsp;由于状态转移的特殊性，答案并不是`dp[n - 1]`，因为最长上升子序列并不一定包含最后一个元素，所以我们还要用一个全局的变量维护一个极大值作为答案返回。

&emsp;&emsp;你以为到这里就结束了吗？当然不是的，题目还给了follow up，时间复杂度是$O(nlogn)$。一提到$nlogn$，又是在数组中，应该要首先考虑到二分查找。但是这里的数字都是乱序，如何做二分呢？我们的目标要把数字分成若干个堆，堆的数量就是答案。怎么分配呢？我们遍历整个数组，从里面拿出元素`num`，然后我们和所有的堆顶元素做比较，找到左边界，然后把`num`放到该堆上，如果找不到，就新建一个堆，把这个元素放进去。

## Solution

&emsp;&emsp;无

------

## Code

### Dynamic Programming

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int size = nums.size();
        vector<int> dp(size, 1);
        
        int result = 1;
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                    result = max(result, dp[i]);
                }
            }
        }
        
        return result;
    }
    
};
```

### Binary Search

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int size = nums.size();
        vector<int> top(size, 0);
        int piles = 0;
        for (int i = 0; i < size; ++i) {
            int poker = nums[i];
            
            // binary
            int left = 0, right = piles;
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (poker <= top[mid]) {
                    right = mid;
                } else {
                    left = mid + 1;
                }
            }
            
            if (left == piles) {
                piles++;
            }
            top[left] = poker;
        }
        
        return piles;
    }
    
};
```



------

## Summary

&emsp;&emsp;这道题目是非常经典的dp，它几乎是所有数组类dp的基础，同时它又有二分查找的方法，结合到了寻找左边界的知识，实实在在是一道好题。这道题目的分享到这里，感谢你的支持！
