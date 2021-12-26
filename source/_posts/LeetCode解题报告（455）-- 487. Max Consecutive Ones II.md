---
title: LeetCode解题报告（455）-- 487. Max Consecutive Ones II
mathjax: true
date: 2021-12-25 23:41:39
---

## Problem

Given a binary array `nums`, return *the maximum number of consecutive* `1`*'s in the array if you can flip at most one* `0`.

<!-- more -->

**Example 1:**

```
Input: nums = [1,0,1,1,0]
Output: 4
Explanation: Flip the first zero will get the maximum number of consecutive 1s. After flipping, the maximum number of consecutive 1s is 4.
```

**Example 2:**

```
Input: nums = [1,0,1,1,0,1]
Output: 4
```

**Constraints:**

- `1 <= nums.length <= 105`
- `nums[i]` is either `0` or `1`.

 

**Follow up:** What if the input numbers come in one by one as an infinite stream? In other words, you can't store all numbers coming from the stream as it's too large to hold in memory. Could you solve it efficiently?

---

## Analysis

&emsp;&emsp;题目给出一个只包含0和1的数字，要求找出连续1的长度，其中可以把某个0翻转成1。题目意思很简单，但一下子好像没什么思路，我们不妨转化一下题目，转化为找最长的子数组，其中最多包含1个0。转化一下是不是有点熟悉的味道了，首先是子数组，然后加了一个限定条件，无脑直接滑动窗口。

&emsp;&emsp;套上模板，用一个变量统计窗口中0的个数，缩小左边界的条件就是当0的个数大于1，然后全局维护一个最大长度即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int size = nums.size();
        int left = 0, right = 0;
        int maxLength = 0;
        int numberOfZero = 0;
        while (right < size) {
            if (nums[right] == 0) {
                numberOfZero++;
            }
            right++;
            
            while (numberOfZero > 1) {
                if (nums[left] == 0) {
                    numberOfZero--;
                }
                left++;
            }
            
            maxLength = max(maxLength, right - left);
        }
        return maxLength;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目实际上就是easy，非常简单的sliding window。这道题目的分享到这里，感谢你的支持！
