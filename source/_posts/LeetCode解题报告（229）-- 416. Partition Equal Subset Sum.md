---
title: LeetCode解题报告（229）-- 416. Partition Equal Subset Sum
tags:
  - LeetCode
mathjax: true
date: 2020-11-27 11:53:20

---

## Problem

Given a **non-empty** array `nums` containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

<!-- more -->

**Example 1:**

```
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

**Example 2:**

```
Input: nums = [1,2,3,5]
Output: false
Explanation: The array cannot be partitioned into equal sum subsets.
```

**Constraints:**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`

------

## Analysis

&emsp;&emsp;这道题目是partition类的题目，之前比较少碰到，所以通过这道题目大概了解一下这类题的解题思路。其实partiton和子序列是非常类似的，因为都是不要求连续的，所以在解题的思路上也是比较类似，优先考虑使用dp。

&emsp;&emsp;回到这道题目，先来看看要求。题目给出一个数组，要求我们判断能否把这个数组分为两个partition，使得这两个partition中的数字之和相等。如果同时考虑两个部分将会非常复杂，一是需要维护两个部分的dp，二是两个partition的元素是互斥的，所以每当一个partition添加一个元素的时候，又会影响到另一个partition。我们能不能只考虑一个呢？这道题目的特点给了我们这个机会，两个部分的和都是一样的，所以这个数值就是整个数组和的一半，因此只要找一个partition即可。如果一个partition的和是总和的一半，那么剩下的那个partition的和也一定是总和的一半。

&emsp;&emsp;上面的分析已经简化了处理的东西，接下来就来看看如何做dp。这里要找的是是否存在子数组使得和为某个特定的值，直接就设`dp[i]`为是否能够组成和为`i`。假设数组的总和为`sum`，那么答案就是`dp[sum/2]`。然后来看动态转移方程，对于某个`nums[i]`来说，`dp[j] = dp[j] || dp[j - nums[i]]`。这条方程不难理解，前者是`dp[j]`本身，后者是指当不包含`nums[i]`时如果能够组成`j - nums[i]`，说明加上后就能组成`j`。

&emsp;&emsp;dp是这道题目最关键的地方，除此之外有一些小技巧也能提升算法的效率。上面说到两个partition的值一定都是总和的一半，也就是说如果总和不能均分一半，就不存在这种情况了，这样也就可以快速判断出不存在的情况了。

------

## Solution

&emsp;&emsp;动态转移方程上面已经给出，这里特别需要注意的是遍历的方向，是从右到左，原因是从左到右的话所有的值都会设为true，dp就失效了。

------

## Code

```c++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if (sum % 2 != 0) {
            return false;
        }
        int target = sum / 2;
        vector<int> dp(target + 1, false);
        dp[0] = true;
        int size = nums.size();
        for (int i = 0; i < size; i++) {
            for (int j = target; j >= nums[i]; j--) {
                dp[j] = dp[j] || dp[j - nums[i]];
            }
        }
        return dp[target];
    }
};
```

------

## Summary

&emsp;&emsp;这类型题目做得比较少，从这道题目开始一点点积累，本质还是依靠dp。这道题目的分享到这里，谢谢！
