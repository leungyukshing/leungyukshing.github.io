---
title: LeetCode解题报告（217）-- 938. Range Sum of BST
tags:
  - LeetCode
mathjax: true
abbrlink: 13178
date: 2020-11-16 00:38:07
---

## Problem

Given the `root` node of a binary search tree, return *the sum of values of all nodes with a value in the range [low, high]*.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/11/05/bst1.jpg)

```
Input: root = [10,5,15,3,7,null,18], low = 7, high = 15
Output: 32
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/11/05/bst2.jpg)

```
Input: root = [10,5,15,3,7,13,18,1,null,6], low = 6, high = 10
Output: 23
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 2 * 104]`.
- `1 <= Node.val <= 105`
- `1 <= low <= high <= 105`
- All `Node.val` are **unique**.

------

## Analysis

&emsp;&emsp;这是一道非常简单的二叉树求和问题，在原有的基础上题目增加了一个范围限制，只有节点的值在给定的范围内才计算，否则忽略。这里我仍然采用递归的方法，在添加本节点的值的时候做一个范围的判断即可。

------

## Solution

&emsp;&emsp;很常规的递归遍历二叉树。

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
    int rangeSumBST(TreeNode* root, int low, int high) {
        if (!root) {
            return 0;
        }
        
        int leftValue = rangeSumBST(root->left, low, high);
        int rightValue = rangeSumBST(root->right, low, high);
        if (root->val >= low && root->val <= high) {
            return leftValue + rightValue + root->val;
        } else {
            return leftValue + rightValue;
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目难度不大，是一道二叉树求和基础上修改的题目。关键是熟练使用递归来遍历二叉树的节点。这道题目的分享到这里，谢谢！
