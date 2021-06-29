---
title: LeetCode解题报告（115）-- 404. Sum of Left Leaves
tags:
  - LeetCode
mathjax: true
abbrlink: 16243
date: 2020-08-26 17:27:11
---

## Problem

Find the sum of all left leaves in a given binary tree.

<!-- more -->

**Example:**

```
    3
   / \
  9  20
    /  \
   15   7

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
```

------

## Analysis

&emsp;&emsp;二叉树关于叶子节点的问题，还是优先考虑递归的做法。这道题目的难点是如何判断出左叶子节点。总体的思路有两个：

1. 在递归的时候带上信息，由父节点告诉子节点是否为左节点，然后子节点判断自己是否为叶子节点，结合递归中的信息来判断自己是否为左叶子节点；
2. 直接在父节点一层判断。

------

## Solution

&emsp;&emsp;上述的两种思路都不复杂，第一种就需要多带一个参数，第二种就可以直接判断，比较简单。

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
    int sumOfLeftLeaves(TreeNode* root) {
        if (!root) {
            return 0;
        }
        if (root->left != NULL && root->left->left == NULL &&root->left->right == NULL) {
            return root->left->val + sumOfLeftLeaves(root->right);
        }
        else {
            return sumOfLeftLeaves(root->left) + sumOfLeftLeaves(root->right);
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题的分析到这里，谢谢！
