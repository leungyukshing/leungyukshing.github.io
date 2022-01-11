---
title: LeetCode解题报告（506）-- 888. Fair Candy Swap
mathjax: true
date: 2022-01-10 22:19:23
---

## Problem

Alice and Bob have a different total number of candies. You are given two integer arrays `aliceSizes` and `bobSizes` where `aliceSizes[i]` is the number of candies of the `ith` box of candy that Alice has and `bobSizes[j]` is the number of candies of the `jth` box of candy that Bob has.

Since they are friends, they would like to exchange one candy box each so that after the exchange, they both have the same total amount of candy. The total amount of candy a person has is the sum of the number of candies in each box they have.

Return a*n integer array* `answer` *where* `answer[0]` *is the number of candies in the box that Alice must exchange, and* `answer[1]` *is the number of candies in the box that Bob must exchange*. If there are multiple answers, you may **return any** one of them. It is guaranteed that at least one answer exists.

<!-- more -->

**Example 1:**

```
Input: aliceSizes = [1,1], bobSizes = [2,2]
Output: [1,2]
```

**Example 2:**

```
Input: aliceSizes = [1,2], bobSizes = [2,3]
Output: [1,2]
```

**Example 3:**

```
Input: aliceSizes = [2], bobSizes = [1,3]
Output: [2,3]
```

**Constraints:**

- `1 <= aliceSizes.length, bobSizes.length <= 104`
- `1 <= aliceSizes[i], bobSizes[j] <= 105`
- Alice and Bob have a different total number of candies.
- There will be at least one valid answer for the given input.

---

## Analysis

&emsp;&emsp;这道题目的背景是Alice和Bob两个人各有若干盒糖，他们需要相互交换各自的一盒糖，使得两个人手上糖的数量相等。这里不存在最优问题，所以不是game theory，因此优先考虑数学做法。采用解方程的方法，两人初始的数量分别是`sum1`和`sum2`，这两个是很容易求出来的。假设`Alice`交换的是`a`，Bob交换的是`b`，那么交换过后我们可以列一个方程：`sum1 - a + b = sum2 - b + a`，把`b`放到一边得到`b = (sum2 - sum1) / 2 + a`。此时我们需要在两个数组中找到满足这个条件的`a`和`b`。

&emsp;&emsp;方法也很简单，我们先把Bob的值都放到一个`unordered_set`中，然后遍历Alice，如果能够存在一个值`x`使得`(sum2 - sum1) / 2 + x`在`unordered_set`中找到，说明等式成立，返回相对应的值即可。如果都找不到最后返回空。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    vector<int> maxSubsequence(vector<int>& nums, int k) {
        vector<int> temp = nums;
        sort(nums.rbegin(), nums.rend());
        unordered_map<int, int> m;
        for (int i = 0; i < k; ++i) {
            ++m[nums[i]];
        }
        
        vector<int> result;
        for (int num: temp) {
            if (m[num]-- > 0) {
                result.push_back(num);
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的难点不在于后面解方程，而在于如何想到是用数学的方法求解，和前面鸡兔同笼的问题有点类似。对于两个人两个变量的题目，如果不存在求极值或最优解的要求，一般都可以先尝试用数学的方法。这道题目的分享到这里，感谢你的支持！

