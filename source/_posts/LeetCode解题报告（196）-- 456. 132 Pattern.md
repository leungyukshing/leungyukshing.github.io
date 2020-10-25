---
title: LeetCode解题报告（196）-- 456. 132 Pattern
tags:
  - LeetCode
mathjax: true
date: 2020-10-25 16:44:19


---

## Problem

Given an array of `n` integers `nums`, a **132 pattern** is a subsequence of three integers `nums[i]`, `nums[j]` and `nums[k]` such that `i < j < k` and `nums[i] < nums[k] < nums[j]`.

Return *true if there is a **132 pattern** in nums, otherwise, return false.*

**Follow up:** The `O(n^2)` is trivial, could you come up with the `O(n logn)` or the `O(n)` solution?

<!-- more -->

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: false
Explanation: There is no 132 pattern in the sequence.
```

**Example 2:**

```
Input: nums = [3,1,4,2]
Output: true
Explanation: There is a 132 pattern in the sequence: [1, 4, 2].
```

**Example 3:**

```
Input: nums = [-1,3,2,0]
Output: true
Explanation: There are three 132 patterns in the sequence: [-1, 3, 2], [-1, 3, 0] and [-1, 2, 0].
```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 104`
- `-109 <= nums[i] <= 109`

------

## Analysis

&emsp;&emsp;题目给定一个数组，判断里面是否存在132这种大小关系的序列。但是注意到这道题目只要求判断是否存在，不需要把所有的情况都返回，自然也就简单不少了。

&emsp;&emsp;三个数的大小关系判断并不容易，思路就是把三个数的比较转为两个数字。在132这种pattern下，1是最小的，这个元素是最好找到的。因为他是最小的，所以我们可以用一个变量记录当前遍历到的最小值。如果当前遍历的元素就是最小值的话，就跳过；否则就从后面开始遍历，看看能否找到一个元素，比当前最小值大，但是比当前的元素要小，如果存在就直接return true了。

&emsp;&emsp;这个思路实际上就是以`i`为分界（作为pattern的2），在前面找到pattern中的1，在后面找到pattern的3。这样就相当于固定了一个元素，双层循环比较后两个元素了。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    bool find132pattern(vector<int>& nums) {
        int size = nums.size();
        int minNumber = INT_MAX;
        
        for (int i = 0; i < size; i++) {
            minNumber = min(minNumber, nums[i]);
            if (minNumber == nums[i]) {
                continue;
            }
            for (int k = size - 1; k > i; k--) {
                if (nums[k] > minNumber && nums[i] > nums[k]) {
                    return true;
                }
            }
        }
        return false;
    }
};
```

------

## Summary

&emsp;&emsp;从这道题目的解法中，我们可以总结出如何处理多个数字互相比较的问题。每多一个数字的比较，就需要多一层的循环，所以为了简化算法，我们要想办法减少需要比较的数字。比如说一些最小值、最大值，我们是可以通过其他的方法确定下来的，这样我们做题就会简单很多。这道题目的分享到这里，谢谢！
