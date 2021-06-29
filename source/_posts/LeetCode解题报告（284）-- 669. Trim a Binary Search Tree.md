---
title: LeetCode解题报告（284）-- 669. Trim a Binary Search Tree
tags:
  - LeetCode
mathjax: true
abbrlink: 63887
date: 2021-02-09 20:32:16
---

## Problem

Given the `root` of a binary search tree and the lowest and highest boundaries as `low` and `high`, trim the tree so that all its elements lies in `[low, high]`. Trimming the tree should **not** change the relative structure of the elements that will remain in the tree (i.e., any node's descendant should remain a descendant). It can be proven that there is a **unique answer**.

Return *the root of the trimmed binary search tree*. Note that the root may change depending on the given bounds.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/09/09/trim1.jpg)

```
Input: root = [1,0,2], low = 1, high = 2
Output: [1,null,2]
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/09/09/trim2.jpg)

```
Input: root = [3,0,4,null,2,null,null,1], low = 1, high = 3
Output: [3,2,null,1]
```

**Example 3:**

```
Input: root = [1], low = 1, high = 2
Output: [1]
```

**Example 4:**

```
Input: root = [1,null,2], low = 1, high = 3
Output: [1,null,2]
```

**Example 5:**

```
Input: root = [1,null,2], low = 2, high = 4
Output: [2]
```

**Constraints:**

- The number of nodes in the tree in the range `[1, 104]`.
- `0 <= Node.val <= 104`
- The value of each node in the tree is **unique**.
- `root` is guaranteed to be a valid binary search tree.
- `0 <= low <= high <= 104`

------

## Analysis

&emsp;&emsp;题目要求对一颗BST剪枝，剪掉不在题目要求范围内的节点。二叉树的问题一般就是通过递归来求解，我们判断节点的值是否在范围内，如果在范围内，则对其左右子树分别递归即可；如果超过范围，分以下情况：

1. 如果小于`low`，说明该节点的左子树也要整个剪掉，直接对其右子树递归；
2. 如果大于`high`，说明该节点的右子树要整个剪掉，直接对其左子树递归。

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
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (!root) {
            return NULL;
        }
        if (root->val < low) {
            return trimBST(root->right, low, high);
        }
        if (root->val > high) {
            return trimBST(root->left, low, high);
        }
        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);
        return root;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是二叉树类的题目的一个小变种，本质上只是对节点的值做判断，然后剪枝。coding方面还是利用比较常见的递归进行求解。这道题这道题目的分享到这里，谢谢您的支持！
