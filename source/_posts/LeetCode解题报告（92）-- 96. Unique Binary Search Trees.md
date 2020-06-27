---
title: LeetCode解题报告（92）-- 96. Unique Binary Search Trees
tags:
  - LeetCode
date: 2020-06-27 11:46:18
mathjax: true


---

## Problem

Given *n*, how many structurally unique **BST's** (binary search trees) that store values 1 ... *n*?

<!-- more -->

**Example:**

```
Input: 3
Output: 5
Explanation:
Given n = 3, there are a total of 5 unique BST's:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

------

## Analysis

&emsp;&emsp;题目要求的是不同搜索二叉树的数量，因为不涉及到二叉树的操作，所以实际上就是找规律的问题了。我们先从零开始对这道题目思考一下，之后我再揭示背后的数学基础。

&emsp;&emsp;从`n=0`开始，空树也是一颗二叉树；当`n=1`，也只有一种情况；当`n=2`，首先用1作为根节点，那么左子树必为空，右子树有一个节点，用2作为根节点，那么右子树必为空，左子树有一个节点；当`n=3`，情况就开始复杂了。用最小的1做根节点和上一种情况类似，用最大的3做根节点也和上一种情况类似，但是用2的情况就有左右子树的问题了。以2为根节点的话，其左子树的根节点可能是所有比2小的数字，其右子树的根节点可能是所有比2大的数字。需要注意到左右子树是相互组合的，所以数量要乘起来。

&emsp;&emsp;经过上面的分析，我们基本把握到规律了，我们需要维护一个`dp[i]`，表示`i`个组成的不同的搜索二叉树的数量。这个规律实际上是叫**卡塔兰数**，它和斐波那契数有相似的地方。直接看它的递推公式：
$$
C_0 = 1 \\C_{n+1} = \sum_{i=0}^{n}C_iC{n-i}
$$

------

## Solution

&emsp;&emsp;根据上面的数列递推表达式，我们先初始化前两项，然后递推计算后面的值，最后返回`dp[n]`即可。

------

## Code

```c++
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1, 0);
        dp[0] = 1;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            for (int j = 0; j < i; j++) {
                dp[i] += dp[j] * dp[i - j - 1];
            }
        }
        return dp[n];
    }
};
```

------

## Summary

 &emsp;&emsp;从这道题目引出的是卡塔兰数列，使用这个数列就能直接解决这道题目了。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
