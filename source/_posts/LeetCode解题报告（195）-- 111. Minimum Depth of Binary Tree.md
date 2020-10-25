---
title: LeetCode解题报告（195）-- 111. Minimum Depth of Binary Tree
tags:
  - LeetCode
mathjax: true
date: 2020-10-25 16:21:42

---

## Problem

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 2
```

**Example 2:**

```
Input: root = [2,null,3,null,4,null,5,null,6]
Output: 5
```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 105]`.
- `-1000 <= Node.val <= 1000`

------

## Analysis

&emsp;&emsp;之前做过求二叉树最大深度的，这道题目就改为求最小深度。本质上都是通过递归调用，自下而上地增加层数，不过在每个节点处，取左右节点的最小值。

------

## Solution

&emsp;&emsp;无

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
    int minDepth(TreeNode* root) {
        if (!root) {
            return 0;
        }
        if (!root->left) {
            return 1 + minDepth(root->right);
        }
        if (!root->right) {
            return 1 + minDepth(root->left);
        }
        return 1 + min(minDepth(root->left), minDepth(root->right));
    }
};
```

------

## Summary

&emsp;&emsp;无论是算法和coding的角度，这道题都是二叉树的基本题目，之前总结果最大深度，这里也把最小深度总结一下，有头有尾，仅此而已。这道题目的分享到这里，谢谢！
