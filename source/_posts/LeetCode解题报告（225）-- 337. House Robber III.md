---
title: LeetCode解题报告（225）-- 337. House Robber III
tags:
  - LeetCode
mathjax: true
abbrlink: 19878
date: 2020-11-23 21:28:21
---

## Problem

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

<!-- more -->

**Example 1:**

```
Input: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

Output: 7 
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```

**Example 2:**

```
Input: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

Output: 9
Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.
```

------

## Analysis

&emsp;&emsp;这道题目是House Robber系列的第三道题目，前两道都是基于数组的，主要的思路是通过dp来求解，但是这道题目画风一变，变成了二叉树，整个思路自然也要调整。题目的要求也很简单，相连的两个节点不能同时被robber。我们先看某个节点的判断逻辑，一般来说有两种选择：

1. 打劫这个节点，所以它的左节点和右节点都不能被打劫。但是它左节点的左右节点、右节点的左右节点都可以被打劫；
2. 不打劫这个节点，打劫它的左节点和右节点。

&emsp;&emsp;按照这个思路，我们只需要计算出每个节点的最优解即可。利用二叉树的性质，通过递归完成简单。

------

## Solution

&emsp;&emsp;这道题目的分析不难，难的是实现。如果按照上述的思路直接进行递归的话，OJ会超时。这是因为在递归过程中，重复计算非常多。因为上面的递归要考虑两层，所以很多节点的计算是重复的。这里提供一个非常简单但是很有价值的解决思路，使用一个map存储已经遍历过的节点和它对应的最优解。这样就能够利用空间换取时间，大大减少计算量。

------

## Code

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int rob(TreeNode* root) {
        unordered_map<TreeNode*, int> m;
        return helper(root, m);
    }
private:
    int helper(TreeNode* root, unordered_map<TreeNode*, int> &m) {
        if (!root) {
            return 0;
        }
        if (m.count(root)) {
            return m[root];
        }
        
        int left = 0, right = 0;
        if (root->left) {
            left = helper(root->left->left, m) + helper(root->left->right, m);
        }
        
        if (root->right) {
            right = helper(root->right->left, m) + helper(root->right->right, m);
        }
        
        int val = max(left + right + root->val, helper(root->left, m) + helper(root->right, m));
        m[root] = val;
        return val;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目再一次证明了解决树的题目递归还是比较实用的方法。从实际的coding也能体会到递归的缺点，大量的重复计算有时候会带来超时的问题。而其中一种比较简便的解决方法就是用map存储已经遍历过的。这道题目的分享到这里，谢谢！
