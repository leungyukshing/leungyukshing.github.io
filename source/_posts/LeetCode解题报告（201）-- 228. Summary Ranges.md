---
title: LeetCode解题报告（201）-- 228. Summary Ranges
tags:
  - LeetCode
mathjax: true
date: 2020-10-28 16:19:58

---

## Problem

You are given a **sorted unique** integer array `nums`.

Return *the **smallest sorted** list of ranges that **cover all the numbers in the array exactly***. That is, each element of `nums` is covered by exactly one of the ranges, and there is no integer `x` such that `x` is in one of the ranges but not in `nums`.

Each range `[a,b]` in the list should be output as:

- `"a->b"` if `a != b`
- `"a"` if `a == b`

<!-- more -->

**Example 1:**

```
Input: nums = [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: The ranges are:
[0,2] --> "0->2"
[4,5] --> "4->5"
[7,7] --> "7"
```

**Example 2:**

```
Input: nums = [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: The ranges are:
[0,0] --> "0"
[2,4] --> "2->4"
[6,6] --> "6"
[8,9] --> "8->9"
```

**Example 3:**

```
Input: nums = []
Output: []
```

**Example 4:**

```
Input: nums = [-1]
Output: ["-1"]
```

**Example 5:**

```
Input: nums = [0]
Output: ["0"]
```

**Constraints:**

- `0 <= nums.length <= 20`
- `-231 <= nums[i] <= 231 - 1`
- All the values of `nums` are **unique**.

------

## Analysis

&emsp;&emsp;题目给定一个数组，要求把相连的数字都压缩成区间，最后返回一个压缩后的数组。思路就按照题目的要求遍历一遍就可以了，记录好每个区间的起点，判断每个元素是否和前一个元素相连，如果相连则不断往后走；如果不相连，则把前面的区间`[start, nums[i - 1]]`加到答案中，然后把`start`设置为`nums[i]`。

------

## Solution

&emsp;&emsp;coding上没有很难的地方，主要是要考虑单个数字和区间的表达形式。如果是单个数字，直接加入到答案中，如果是区间，则需要加上特定的格式。还有对于最后一个区间或数字，需要在遍历完后再判断一次，否则会漏掉。

------

## Code

```c++
class Solution {
public:
    vector<string> summaryRanges(vector<int>& nums) {
        vector<string> result;
        int size = nums.size();
        if (size == 0) {
            return result;
        }
        
        int start = nums[0];
        for (int i = 1; i < size; i++) {
            if (nums[i] != nums[i - 1] + 1) {
                if (nums[i - 1] != start) {
                    result.push_back(to_string(start) + "->" + to_string(nums[i - 1]));
                } else {
                    result.push_back(to_string(start));
                }
                start = nums[i];
            }
        }
        if (start == nums[size - 1]) {
            result.push_back(to_string(start));
        } else {
            result.push_back(to_string(start) + "->" + to_string(nums[size - 1]));
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难度不大，是对数组最基本的操作，这里分享是为了能够熟练地应对这类型的题目。这道题目的分享到这里，谢谢！
