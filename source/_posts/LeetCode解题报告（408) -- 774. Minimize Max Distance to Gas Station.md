---
title: LeetCode解题报告（408) -- 774. Minimize Max Distance to Gas Station
mathjax: true
date: 2021-09-17 17:58:24
---

## Problem

You are given an integer array `stations` that represents the positions of the gas stations on the **x-axis**. You are also given an integer `k`.

You should add `k` new gas stations. You can add the stations anywhere on the **x-axis**, and not necessarily on an integer position.

Let `penalty()` be the maximum distance between **adjacent** gas stations after adding the `k` new stations.

Return *the smallest possible value of* `penalty()`. Answers within `10-6` of the actual answer will be accepted.

<!-- more -->

**Example 1:**

```
Input: stations = [1,2,3,4,5,6,7,8,9,10], k = 9
Output: 0.50000
```

**Example 2:**

```
Input: stations = [23,24,36,39,46,56,57,65,84,98], k = 1
Output: 14.00000
```

**Constraints:**

- `10 <= stations.length <= 2000`
- `0 <= stations[i] <= 108`
- `stations` is sorted in a **strictly increasing** order.
- `1 <= k <= 106`

------

## Analysis

&emsp;&emsp;题目给出一个数组`stations`，每个位置`stations[i]`标明位置，同时给出`k`待插入加油站的位置，要求计算出，插入了这个`k`个加油站后，使得加油站相互之间距离的最大值要最小。一开始的想法是贪心的思路，每次都选最大的空隙加进去1个，直至没有新的加油站可以插入。但是这个思路并不work，因为当有一个非常大的空隙，实际上我们需要往里面插入两个加油站，贪心的策略并不能cover这种情况。于是我们就需要换一种算法，这里用到了二分答案。二分答案这种方法并不是很常用，但是很多情况下能够快速解决复杂的题目，其基本思路就是通过对答案区间进行二分查找，对查找到的每一个答案，根据限制条件计算出需要如何调整，最终收敛到答案。

&emsp;&emsp;看回题目，要求的是最后最大的一个间隙的长度，所以我们就来二分这个长度。题目给出了这个间隙的取值范围是`[0, 1e6]`。我们来看如何做二分，当我们计算出`mid`时，这个就是当前待选的区间长度，然后我们遍历一遍`stations`，统计出间隙比`mid`大的个数`count`。我们需要通过`count`去控制二分选择左半边还是右半边。这个`count`代表的是当前有多少个间隙比候选的答案要大，如果：

+ `count > k`：说明当前有超过`k`个间隙比`mid`大，此时加油站不够了，所以说明`mid`选小了，二分就要选择右边部分；
+ `count <= k`：说明当前少于`k`个间隙比`mid`大，此时说明加油站多了，所以说明`mid`选大了，二分就要选择左边部分；

&emsp;&emsp;弄清楚二分的原理后，coding就很简单了。

## Solution

&emsp;&emsp;无

------

## Code

```c++
class Solution {
public:
    double minmaxGasDist(vector<int>& stations, int k) {
        const double EPS = 1e-6;
        double left = 0, right = 1e8;
        
        while (right - left > EPS) {
            double mid = (left + right) / 2;
            if (check(stations, k, mid)) {
                right = mid;
            } else {
                left = mid;
            }
        }
        return left;
    }
private:
    bool check(vector<int>& stations, int k, double mid) {
        int count = 0;
        int size = stations.size();
        for (int i = 1; i < size; i++) {
            count += (stations[i] - stations[i - 1]) / mid;
        }
        return count <= k;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目虽然是hard，但是如果对二分答案熟悉的朋友，应该非常容易就能解决。这道题目的分享到这里，感谢你的支持！

