---
title: LeetCode解题报告（189）-- 189. Rotate Array
tags:
  - LeetCode
mathjax: true
abbrlink: 49925
date: 2020-10-16 10:57:49
---

## Problem

Given an array, rotate the array to the right by *k* steps, where *k* is non-negative.

**Follow up:**

- Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.
- Could you do it in-place with O(1) extra space?

<!-- more -->

**Example 1:**

```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

**Example 2:**

```
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
```

**Constraints:**

- `1 <= nums.length <= 2 * 104`
- `-231 <= nums[i] <= 231 - 1`
- `0 <= k <= 105`

------

## Analysis

&emsp;&emsp;看这道题目的时候有没有似曾相识的感觉？之前我们做过一道Rotate List的题目，是针对链表的，而这道题目是针对数组的。做旋转的操作在链表上做明显比在数组中要简单，因为对于链表只需要确定好头之后，其余的都是连接好的，但是对于数组来说，挪动数字并不是一件容易的事情。

&emsp;&emsp;结合题目的要求，空间复杂度是$O(1)$，所以只能在原来的数组上进行操作。虽然有了更多的限制，但是思路也更容易确定了，在这个限制条件下，考虑的是怎么样**交换元素的位置**。以example1为例，我们先来分析一下，通过对比旋转前后元素的位置，找一下规律。

- 下标为0的数 -> 下标为3
- 下标为3的数 -> 下标为6
- 下标为6的数 -> 下标为2

&emsp;&emsp;以此类推，会发现这是一个闭环。下一个下标`nextIdx`和当前下标`idx`的关系为`nextIdx = (idx + k) % size`。根据这个公式，当我们循环`size`次后，数组刚好就完成了一次旋转。而利用下标做交换的空间复杂度是常数级别，符合题目要求。

&emsp;&emsp;别高兴得太早，一跑example2就发现有问题了，只交换了下标为0和2的元素，而下标为1和3的元素没有动过。这个问题要怎么解决呢？根据上面的分析，我们一共是做`size`次操作的，那么example2这种情况实际上是数组内部的交换有两个独立的循环。为了打破这种循环，当第一个循环完成的时候，我们把`idx`加1，这样就能跳入到下一个循环了。这里为什么是加1？因为当旋转的位置大于1的时候，第一个循环完成之后，他隔壁的元素一定是在下一个循环的，所以加一就可以进入第二个循环。

------

## Solution

&emsp;&emsp;在循环的时候使用多一个遍历保存中间结果就可以了。为了解决example2的情况，额外加多一个`start`遍历去做循环的控制。

------

## Code

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int size = nums.size();
        if (k % size == 0) {
            return;
        }
        k %= size;
        int start = 0;
        int idx = 0;
        int temp = nums[idx];
        for (int i = 0; i < size; i++) {
            int nextIdx = (idx + k) % size;
            int ii = nums[nextIdx];
            nums[nextIdx] = temp;
            temp = ii;
            idx = nextIdx;
            if (idx == start) {
                idx = ++start;
                temp = nums[idx];
            }
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目体现了数组和链表最大的不同，解决数组问题的时候要优先从下标下手，找到下标的规律。同时，解决数组内多个独立循环也是从下标入手解决的。这都说明了在不考虑额外空间的情况下，下标是对付数组类型题目比较好的方法。这道题目的分享到这里，谢谢！
