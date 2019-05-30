---
title: LeetCode解题报告（46）-- 283. Move Zeroes
tags:
  - LeetCode
abbrlink: 1246
date: 2019-04-06 00:46:26
---
## Problem

Given an array `nums`, write a function to move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

<!-- more -->

**Example:**

```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Note:**

1. You must do this **in-place** without making a copy of the array.
2. Minimize the total number of operations.

---

## Analysis

&emsp;&emsp;这道题要求我们将一个数组中的0元素放到后面，将其他元素放到前面，并且非零元素之间的相对顺序不能改变。这本身是一道很简单的题目，但是Note中要求我们不使用额外的内存空间，因此就需要在这个数组中直接操作了。

&emsp;&emsp;这种数组内的元素交换问题有一种普遍的解法——**双指针**。使用一个指针指向0元素，另一个指针指向非0元素。这样做的思路是，我们使用0元素指针来告诉我们该位置上的0是可以被替换掉的，使用非0元素指针来告诉我们该位置上的元素是需要替换的。

&emsp;&emsp;于是，这个算法的思路就很清晰了。整个过程中，我需要维护两个指针：

+ 当非0元素指针指向0元素时，该指针往后移，寻求下一个需要换位的元素。
+ 其余情况下，交换这两个指针位置上的元素，交换后分别向前一格，也就是说，0元素指针寻找下一个可以被交换的0，而非零元素指针寻找下一个需要交换的元素。

&emsp;&emsp;最后是确认循环的边界，因为题目的要求是处理所有的非零元素，也就是说，非零元素指针如果遍历完这个数组，那么就说明我们已经处理完毕，所以终止条件就是：**非零元素指针到达数组边界**。

---

## Solution

&emsp;&emsp;使用两个变量来存放下表作为指针，根据上述思路在每个循环中判断即可。

---

## Code

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int zero_ptr = 0;
        int none_zero_ptr = 0;
        while (none_zero_ptr < nums.size()) {
            if (nums[none_zero_ptr] == 0) {
                none_zero_ptr++;
            }
            else {
                int tmp = nums[none_zero_ptr];
                nums[none_zero_ptr] = nums[zero_ptr];
                nums[zero_ptr] = tmp;
                zero_ptr++;
                none_zero_ptr++;
            }
        }
    }
};
```

---

## Summary

&emsp;&emsp;这种在数组内部交换元素的题目非常适合使用双指针，这道题本身难度不大，但是提供了一种普遍的思路，还是值得总结的。谢谢您的支持！