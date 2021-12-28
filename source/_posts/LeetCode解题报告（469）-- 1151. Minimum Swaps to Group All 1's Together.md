---
title: LeetCode解题报告（469）-- 1151. Minimum Swaps to Group All 1's Together
mathjax: true
date: 2021-12-28 14:12:06
---

## Problem

Given a binary array `data`, return the minimum number of swaps required to group all `1`’s present in the array together in **any place** in the array.

<!-- more -->

**Example 1:**

```
Input: data = [1,0,1,0,1]
Output: 1
Explanation: There are 3 ways to group all 1's together:
[1,1,1,0,0] using 1 swap.
[0,1,1,1,0] using 2 swaps.
[0,0,1,1,1] using 1 swap.
The minimum is 1.
```

**Example 2:**

```
Input: data = [0,0,0,1,0]
Output: 0
Explanation: Since there is only one 1 in the array, no swaps are needed.
```

**Example 3:**

```
Input: data = [1,0,1,0,1,0,0,1,1,0,1]
Output: 3
Explanation: One possible solution that uses 3 swaps is [0,0,0,0,0,1,1,1,1,1,1].
```



**Constraints:**

- `1 <= data.length <= 105`
- `data[i]` is either `0` or `1`.

---

## Analysis

&emsp;&emsp;题目给出一个二值数组，问最少通过多少个swap操作能够把所有的1放在一起。首先我们可以知道，1的数量就是最后连续1的长度，我们可以把这个先计算出来，记为`totalOnes`。然后就使用滑动窗口，我们把窗口的size定义为`totalOnes`，对于每个窗口维护一个`ones`，表示窗口中有多少个1，那么我们要的就是最多的1，维护一个极大值`maxOnes`，最后用`totalOnes - maxOnes`就是最少的swap次数。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int minSwaps(vector<int>& data) {
        int totalOnes = 0;
        for (int &num: data) {
            totalOnes += num;
        }
        
        int left = 0, right = 0, size = data.size();
        int ones = 0, maxOnes = 0;
        while (right < size) {
            if (data[right++]) {
                ones++;
            }
            
            while (right - left > totalOnes) {
                if (data[left++]) {
                    ones--;
                }
            }
            
            maxOnes = max(maxOnes, ones);
        }
        
        return totalOnes - maxOnes;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是使用sliding window解决的经典题目，直接套用即可，难度不大。这道题目的分享到这里，感谢你的支持！
