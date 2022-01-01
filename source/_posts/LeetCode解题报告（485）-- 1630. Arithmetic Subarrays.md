---
title: LeetCode解题报告（485）-- 1630. Arithmetic Subarrays
mathjax: true
date: 2022-01-01 13:42:38
---

## Problem

A sequence of numbers is called **arithmetic** if it consists of at least two elements, and the difference between every two consecutive elements is the same. More formally, a sequence `s` is arithmetic if and only if `s[i+1] - s[i] == s[1] - s[0] `for all valid `i`.

For example, these are **arithmetic** sequences:

```
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
```

The following sequence is not **arithmetic**:

```
1, 1, 2, 5, 7
```

You are given an array of `n` integers, `nums`, and two arrays of `m` integers each, `l` and `r`, representing the `m` range queries, where the `ith` query is the range `[l[i], r[i]]`. All the arrays are **0-indexed**.

Return *a list of* `boolean` *elements* `answer`*, where* `answer[i]` *is* `true` *if the subarray* `nums[l[i]], nums[l[i]+1], ... , nums[r[i]]` *can be **rearranged** to form an **arithmetic** sequence, and* `false` *otherwise.*

<!-- more -->

**Example 1:**

```
Input: nums = [4,6,5,9,3,7], l = [0,0,2], r = [2,3,5]
Output: [true,false,true]
Explanation:
In the 0th query, the subarray is [4,6,5]. This can be rearranged as [6,5,4], which is an arithmetic sequence.
In the 1st query, the subarray is [4,6,5,9]. This cannot be rearranged as an arithmetic sequence.
In the 2nd query, the subarray is [5,9,3,7]. This can be rearranged as [3,5,7,9], which is an arithmetic sequence.
```

**Example 2:**

```
Input: nums = [-12,-9,-3,-12,-6,15,20,-25,-20,-15,-10], l = [0,1,6,4,8,7], r = [4,4,9,7,9,10]
Output: [false,true,false,false,true,true]
```



**Constraints:**

- `n == nums.length`
- `m == l.length`
- `m == r.length`
- `2 <= n <= 500`
- `1 <= m <= 500`
- `0 <= l[i] < r[i] < n`
- `-105 <= nums[i] <= 105`

---

## Analysis

&emsp;&emsp;题目给出了一个数组`nums[i]`，还有`l`和`r`，`l[i]`和`r[i]`组成一个区间，要求返回每一个区间是否是一个等差数列。所以这道题目的本质就是检查某个数组是否等差数列，这是一个通用的方法，在碰到其他的等差数列问题时也可以采取类似的方法。

&emsp;&emsp;首先找出数列的最大值`mx`和最小值`mi`，如果两者相同说明里面的元素都是一样的，等差为0，所以也是等差数列。如果两者不相等，我们尝试计算等差`diff = (mx - mi) / len`，但是在计算前要检查能否被整除，如果不能被整除说明不存在等差。如果存在等差的话，还不能说明这就是一个等差数列，还要逐个数字检查，检查`nums[i] - mi`能否整除`diff`，同时相同的数字还不能出现两遍（等差为0的情况之前已经判断过了，如果这里存在两个数字相同，说明不符合要求）。如果所有的检查都没有问题的话，才能认为这是一个等差数列。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    vector<bool> checkArithmeticSubarrays(vector<int>& nums, vector<int>& l, vector<int>& r) {
        int size = l.size();
        vector<bool> result(size, false);
        
        for (int i = 0; i < size; ++i) {
            int left = l[i], right = r[i], len = right - left + 1;
            int mx = INT_MIN, mi = INT_MAX;
            for (int j = left; j <= right; ++j) {
                mx = max(mx, nums[j]);
                mi = min(mi, nums[j]);
            }
            
            if (mx == mi) {
                result[i] = true;
                continue;
            }
            
            if ((mx - mi) % (len - 1)) {
                continue;
            }
            vector<bool> diffs(len, false);
            int diff = (mx - mi) / (len - 1);
            int j = left;
            for (; j <= right; ++j) {
                if ((nums[j] - mi) % diff || diffs[(nums[j] - mi) / diff]) {
                    break;
                }
                diffs[(nums[j] - mi) / diff] = true;
            }
            
            if (j > right) {
                result[i] = true;
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的重点就是怎么判断一个等差数列，我认为整理一下这个思路是很有用的。这道题目的分享到这里，感谢你的支持！

