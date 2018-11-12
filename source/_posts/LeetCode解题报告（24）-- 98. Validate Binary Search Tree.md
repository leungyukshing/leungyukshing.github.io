---
title: LeetCode解题报告（24）-- 98. Validate Binary Search Tree
tags:
  - LeetCode
abbrlink: 54524
date: 2018-10-22 18:54:55
---
## Problem
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:
  + The left subtree of a node contains only nodes with keys **less than** the node's key.
  + The right subtree of a node contains only nodes with keys **greater than** the node's key.
  + Both the left and right subtrees must also be binary search trees.
<!-- more -->

**Example 1**:
```
Input:
    2
   / \
  1   3
Output: true
```

**Example 2**:
```
Input:
    5
   / \
  1   4
     / \
    3   6
Output: false
Explanation: The input is: [5,1,4,null,null,3,6]. The root node's value
             is 5 but its right child's value is 4.
```

## Analysis
&emsp;&emsp;这道题涉及到了二叉树的内容，题目给出一颗二叉树，要求我们判断这颗二叉树是否满足题目的要求。要求为：左节点的值小于当前节点，右节点的值大于当前节点（实质上是二叉搜索树的定义）。
&emsp;&emsp;关于二叉树的题目大多都可以通过递归解决，这题也不例外。对于每一个节点，验证它的值是否在区间`[small, big]`中。对左子树而言，`big`值是这颗左子树的根节点的值，对于右子树而言，`small`值是这颗右子树的根节点的值。因此判断出错误情况直接返回`false`就可以了。

## Solution
&emsp;&emsp;使用递归实现，注意判断的条件`(root->val <= small || root->val >= big)`，不需要担心处理左子树的时候`big`的情况以及处理右子树时`small`的情况。

## Code
```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
       return recursive(root, LONG_MIN, LONG_MAX);
    }
private:
    bool recursive(TreeNode* root, long small, long big) {
        if (root == NULL)
            return true;
        if (root->val <= small || root->val >= big)
            return false;
        return recursive(root->left, small, root->val) && recursive(root->right, root->val, big);
    }
};
```
**运行时间**：约4ms，超过100%的CPP代码。

## Summary
&emsp;&emsp;在处理树，尤其是二叉树的时候，要充分利用其递归定义的属性，使用递归的方式来处理，这样能大大减少代码量。但是要理解递归是比较困难的，最好是截取一部分在纸上写出递归的过程，这样就会容易理解。本题的分析到这里，谢谢！
