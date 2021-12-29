---
title: LeetCode解题报告（471）-- 1481. Least Number of Unique Integers after K Removals
mathjax: true
date: 2021-12-29 15:46:52
---

## Problem

Given an array of integers `arr` and an integer `k`. Find the *least number of unique integers* after removing **exactly** `k` elements**.**

<!-- more -->

**Example 1:**

```
Input: arr = [5,5,4], k = 1
Output: 1
Explanation: Remove the single 4, only 5 is left.
```

**Example 2:**

```
Input: arr = [4,3,1,1,3,3,2], k = 3
Output: 2
Explanation: Remove 4, 2 and either one of the two 1s or three 3s. 1 and 3 will be left.
```



**Constraints:**

- `1 <= arr.length <= 10^5`
- `1 <= arr[i] <= 10^9`
- `0 <= k <= arr.length`

---

## Analysis

&emsp;&emsp;题目给出一个数组`arr`还有一个`k`，题目问移除`k`个数字后，剩下的最少unique number是多少个？因为我们要剩下的unique number数量最少，所以这里可以用贪心的做法，我们每次都去掉frequency最少的number，这样就能最快去掉一种数字。

&emsp;&emsp;先用一个`unordered_map`统计频率，然后把频率都放到priority queue中，从小到大排列。然后每次都队头拿出，`k`减去频率后如果大于或等于0，说明1种数字被去除掉了，所以要把频率从队列中pop出；否则，说明`k`已经不够了，不能去除掉任何的数字，最后返回queue的size即可。

## Solution

&emsp;&emsp;无。

------

## Code

```c++
class Solution {
public:
    int findLeastNumOfUniqueInts(vector<int>& arr, int k) {
        unordered_map<int, int> freq;
        for (int num: arr) {
            ++freq[num];
        }
        
        priority_queue<int, vector<int>, greater<int>> pq;
        for (auto &[k, v]: freq) {
            pq.push(v);
        }
        
        while (k > 0) {
            k -= pq.top();
            if (k >= 0) {
                pq.pop();
            }
        }
        return pq.size();
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难度不大，主要的思路是统计频率已经贪心算法，贪心算法很常见的套路就是结合priority queue。这道题目的分享到这里，感谢你的支持！

