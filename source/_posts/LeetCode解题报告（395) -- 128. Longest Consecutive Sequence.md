---
title: LeetCode解题报告（395) -- 128. Longest Consecutive Sequence
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

&emsp;&emsp;题目给出了一个数组，要求找出最大的连续元素的长度。因为不需要保留原有的位置关系，所以直接排序，方便判断是否连续。判断的逻辑并不困难，有两个点要特别注意：

1. 当`nums[i] == nums[i - 1]`时，长度不算入其中，但是并不影响连续性；
2. 当`i = size - 1`时，说明到了最后一个元素，需要更新一次极大值。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int result = 1, length = 1;
        int size = nums.size();
        if (size == 0) {
            return 0;
        }
        for (int i = 1; i < size; i++) {
            if (nums[i] - nums[i - 1] == 1) {
                length++;
            } else if (nums[i] == nums[i - 1]) {
                
            } else {
                result = max(result, length);
                length = 1;
            }
            if (i == size - 1) {
                result = max(result, length);
            }
        }
        
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题比较简单，主要依靠排序判断元素的连续性，处理好两个边界条件就可以了。这道题目的分享到这里，感谢你的支持！
