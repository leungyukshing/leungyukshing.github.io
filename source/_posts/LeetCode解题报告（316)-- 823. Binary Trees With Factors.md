---
title: LeetCode解题报告（316)-- 823. Binary Trees With Factors
tags:
  - LeetCode
mathjax: true
date: 2021-03-14 00:48:28

---

## Problem

Given an array of unique integers, `arr`, where each integer `arr[i]` is strictly greater than `1`.

We make a binary tree using these integers, and each number may be used for any number of times. Each non-leaf node's value should be equal to the product of the values of its children.

Return *the number of binary trees we can make*. The answer may be too large so return the answer **modulo** `109 + 7`.

<!-- more -->

**Example 1:**

```
Input: arr = [2,4]
Output: 3
Explanation: We can make these trees: [2], [4], [4, 2, 2]
```

**Example 2:**

```
Input: arr = [2,4,5,10]
Output: 7
Explanation: We can make these trees: [2], [4], [5], [10], [4, 2, 2], [10, 2, 5], [10, 5, 2].
```

**Constraints:**

- `1 <= arr.length <= 1000`
- `2 <= arr[i] <= 109`

------

## Analysis

&emsp;&emsp;题目给出一个数组，要求使用这个数组中的数字构成二叉树，这颗二叉树的每个非叶子节点的值是其两个子节点的乘积，问能组成多少颗这样的二叉树。先从宏观上看看思路，处理这类题目无非两种方法，第一种是构造出所有符合要求的二叉树，然后计算出总数，第二种是不用构造出每一颗二叉树，使用dp。第一种方法大概率行不通了，因为题目提醒答案可能很大，所以用递归的话大概率超时。然后我们就来看dp的方法，按照题目直接无脑定义`dp[i]`为以节点`i`为根节点时，能够成符合题目要求的二叉树数量。

&emsp;&emsp;然后来看转移方程，假设根节点是`i`，左子节点是`j`，右子节点是`k`，那么`dp[i] = dp[j] * dp[k] + 1`。为什么是相乘？因为左子树有x种情况，右子树有y种情况，那么组合起来就是`x * y`种情况了。为什么要加1？因为如果没有左、右子树，那么单独一个根节点也是符合题目要求的。

&emsp;&emsp;现在问题转化为如何找到左子节点`j`和右子节点`k`，首先关注左子节点`j`，因为在知道根节点`i`和左子节点`j`的情况下，就可以算出右子节点`k = i / j`。`j`的取值是肯定要比`i`要小的（否则无法相乘得到根节点），所以可以先对数组进行排序，从小到大遍历。在确定根节点`i`后，遍历每一个比`i`要小的`j`，然后：

1. 先判断`i`能不能整除`j `，如果不能整除，则说明`j`不可能是`i`其中的一个子节点；
2. 如果可以整除，算一下另一个因子`i / j`在不在dp的数组中，如果在的话，则更新dp值。

&emsp;&emsp;完成之后把所有的dp值都加起来，就是能够满足题目要求的二叉树总数。

------

## Solution

&emsp;&emsp;上述第二步有个问题，怎么通过另一个因子的值去找它在不在dp数组呢？这里的数组需要做一个小变形，用一个hash map存储，key是数值，value是对应的dp值，这是为了方便根据因子的值找到对应的dp值。

------

## Code

```c++
class Solution {
public:
    int numFactoredBinaryTrees(vector<int>& arr) {
        const long M = 1e9 + 7;
        int size = arr.size();
        sort(arr.begin(), arr.end());
        unordered_map<int, long> dp;
        for (int i = 0; i < size; i++) {
            dp[arr[i]] = 1;
            for (int j = 0; j < i; j++) {
                if (arr[i] % arr[j] == 0 && dp.count(arr[i] / arr[j])) {
                    dp[arr[i]] = (dp[arr[i]] + dp[arr[j]] * dp[arr[i] / arr[j]]) % M;
                }
            }
        }
        long result = 0;
        for (auto a: dp) {
            result = (result + a.second) % M;
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是二叉树的一道比较新颖的题目，是通过dp的方法求解的。题目还是有一定难度，在找状态转移方程时需要对常规的dp数组进行变化。这道题目的分享到这里，感谢你的支持！
