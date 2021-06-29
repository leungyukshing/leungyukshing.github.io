---
title: LeetCode解题报告（365) -- 665. Non-decreasing Array
tags:
  - LeetCode
mathjax: true
abbrlink: 35732
date: 2021-05-05 15:06:15
---

## Problem

Given an array `nums` with `n` integers, your task is to check if it could become non-decreasing by modifying **at most one element**.

We define an array is non-decreasing if `nums[i] <= nums[i + 1]` holds for every `i` (**0-based**) such that (`0 <= i <= n - 2`).

<!-- more -->

**Example 1:**

```
Input: nums = [4,2,3]
Output: true
Explanation: You could modify the first 4 to 1 to get a non-decreasing array.
```

**Example 2:**

```
Input: nums = [4,2,1]
Output: false
Explanation: You can't get a non-decreasing array by modify at most one element.
```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 104`
- `-105 <= nums[i] <= 105`

------

## Analysis

&emsp;&emsp;题目给出一个数组要求在允许修改一个数值的情况下，判断这个数组是不是非递减的。从前往后遍历通过`nums[i - 1] > nums[i]`能判断出后面的数比前一个小，不符合题目的要求。

&emsp;&emsp;一开始我的想法是直接判断上面出现的情况的次数，如果出现的次数超过一次则返回false，但是其实是有问题的。比如`[3, 4, 2, 3]`这个数组，虽然只有4和2这一个地方满足了上面的条件，但实际上是不能通过只改变一个数字的值使得数组变成非递减的。因为无论把4修改成什么值，都无法做到。所以这里不能盲目地只判断出现的次数，还需要把实际更新的操作也做了，然后一直往后遍历下去。

&emsp;&emsp;如何更新数值就是这道题目的难点所在。以题目给出的Example1为例，应该是把4改成2，最后数组变成`[2,2,3]`；再看另一个例子`[-1,4,2,3]`，这里也是需要把4修改成为，最后数组变成`[-1,2,2,3]`；最后看一个例子`[2,3,3,2,4]`，这里并不是把3改成2，而是把2修改成3，最后数组变成`[2,3,3,3,4]`。通过这三个例子，可以发现，前两个例子都是把`nums[i]`修改为`nums[i - 1]`，而最后一个例子是把`nums[i - 1]`修改成`nums[i]`。

&emsp;&emsp;是什么导致了上面两种不同的修改方式呢？是`nums[i - 2]`这个数。Example1是因为前面没有数字了，Example2是因为`nums[i - 2] < nums[i]`，Example2是因为`nums[i - 2] > nums[i]`，所以我们需要根据这个大小关系，选择不同的更新方法。

&emsp;&emsp;同时维护一个count值，初始化为1，当完成一次更新操作后，减1，如果后面还有更新操作，则说明不满足题目的意思，直接return false。

------

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        int count = 1;
        int size = nums.size();
        for (int i = 1; i < size; i++) {
            if (nums[i - 1] > nums[i]) {
                if (count <= 0) {
                    return false;
                }
                if (i == 1 || nums[i] >= nums[i - 2]) {
                    nums[i - 1] = nums[i];
                } else {
                    nums[i] = nums[i - 1]; 
                }
                count--;
            }
        }
        return true;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目虽然看上去意思很简单，但是实际打起来还是涉及到了一些细节。这道题目的分享到这里，感谢你的支持！
