---
title: LeetCode解题报告（203）-- 673. Number of Longest Increasing Subsequence
tags:
  - LeetCode
mathjax: true
date: 2020-10-31 11:48:02


---

## Problem

Given an integer array `nums`, return *the number of longest increasing subsequences.*

<!-- more -->

**Example 1:**

```
Input: nums = [1,3,5,4,7]
Output: 2
Explanation: The two longest increasing subsequences are [1, 3, 4, 7] and [1, 3, 5, 7].
```

**Example 2:**

```
Input: nums = [2,2,2,2,2]
Output: 5
Explanation: The length of longest continuous increasing subsequence is 1, and there are 5 subsequences' length is 1, so output 5.
```

**Constraints:**

- `0 <= nums.length <= 2000`
- `-106 <= nums[i] <= 106`

------

## Analysis

&emsp;&emsp;前几天碰到的题目是求具有132模式的子序列的个数，这里要求的是最长递增子序列的个数，看起来都是类似的，都是求一个极值，自然也就先考虑使用dp。但是这道题目难的地方也在于怎么去用dp，因为我们要找的是**最长且递增**的，所以在遍历的时候要把长度信息记录下。

&emsp;&emsp;如果把dp设为**dp[i]为到第i个位置结尾的最长子序列个数**，会有一定的问题，这样设置我们就不知道它的长度。所以为了同时考虑长度信息和个数信息，我们用两个dp状态：

- `len[i]`：以`nums[i]`结尾的最长递增子序列的长度；
- `count[i]`：以`nums[i]`结尾的最长递增子序列的个数。

&emsp;&emsp;有了以上两种状态，对于某一个已经遍历过的`i`，我们就能够知道它的长度和个数，接下来就来看看状态怎么转移。从头开始遍历，用`i`作为下标；然后对于每个`i`，再从头遍历到`i`，用`j`作为下标，这是子序列问题常见的做法，因为后面的长度和个数都是基于前面的计算的。

- 当`nums[i] <= nums[j]`的时候，不满足递增的要求，直接skip；
- 当`len[i] < len[j] + 1`的时候，说明前面有最长的子序列，直接把长度和个数都赋值给第`i`个元素；
- 当`len[i] = len[j] + 1` 的时候，说明前面已经处理过相同的长度了，这个条件说明是第二次出现，所以直接把`j`的数量加到`i`即可。

&emsp;&emsp;前面两种情况都比较好理解，这里我用Example1为例子解释一下第三种情况。`[1,3,5,4,7]`在刚开始处理到`i`为4的时候，`len[4] = 1`，`len[2] = 3`，`len[3] = 3`。当`j = 2`的时候，因为`len[4] < len[2] + 1`，所以进入了第二种情况，更新完后`len[4] = 4`。当`j = 3` 的时候，因为`len[4] = len[3] + 1`，这就意味着长度为3的最长子序列在之前已经处理过了，而且当前处理过的最长的子序列长度也是3，所以这种情况应该纳入到计算中，因此`count[4] += count[3]`。

&emsp;&emsp;最后在每处理完一个数字后，都需要用较大的值更新答案。因为要求的是最长的递增子序列个数，所以我们要记录下最大的长度。

------

## Solution

&emsp;&emsp;按照上述的状态转移思路coding即可，需要注意`len`和`count`两个数组都应该初始化为1，因为单个数字本身就是一个递增的子序列。

------

## Code

```c++
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        int result = 0, maxLen = 0, size = nums.size();
        vector<int> len(size, 1), count(size, 1);
        
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] <= nums[j]) {
                    // no incremental
                    continue;
                }
                if (len[i] == len[j] + 1) {
                    count[i] += count[j];
                } else if (len[i] < len[j] + 1) {
                    len[i] = len[j] + 1;
                    count[i] = count[j];
                }
            }
            if (maxLen == len[i]) {
                result += count[i];
            } else if (maxLen < len[i]) {
                maxLen = len[i];
                result = count[i];
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是比较复杂的dp了，有两个状态控制。题目的特点就是最长和递增，然后要求计算数量，当单个状态转移没有办法提供足够的信息的时候，我们就可以考虑多用一个状态转移方程。这道题目的分享到这里，谢谢！
