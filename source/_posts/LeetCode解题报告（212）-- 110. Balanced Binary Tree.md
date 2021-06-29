---
title: LeetCode解题报告（212）-- 110. Balanced Binary Tree
tags:
  - LeetCode
mathjax: true
abbrlink: 17111
date: 2020-11-08 22:43:10
---

## Problem

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

> a binary tree in which the left and right subtrees of *every* node differ in height by no more than 1.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: true
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

```
Input: root = [1,2,2,3,3,null,null,4,4]
Output: false
```

**Example 3:**

```
Input: root = []
Output: true
```

------

## Analysis

&emsp;&emsp;这道题目要求判断一颗二叉树是否是balanced的，题目对balanced给出的定义是**左右子树的高度相差不超过1**。与高度相关的毫无疑问就是递归遍历了，相信计算高度已经难不到大家。对于某个节点，拿到左右子树的高度后判断是否满足balanced条件。只要有一个节点不满足条件就返回false。

------

## Solution

&emsp;&emsp;这道题目有个巧妙的地方是在递归中如何完成判断。在循环中完成判断是很容易的，直接break或者return就可以，但是递归就有点麻烦。这里我多用了一个变量`flag`去记录是否退出，一旦有不满足balanced的情况出现，就设置为true，后续的递归也就不用处理了。

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
    bool isBalanced(TreeNode* root) {
        bool flag = false;
        int result = helper(root, flag);
        return !flag;
    }
private:
    int helper(TreeNode* root, bool& flag) {
        if (flag) {
            return -1;
        }
        if (!root) {
            return 0;
        }
        int leftHeight = helper(root->left, flag);
        int rightHeight = helper(root->right, flag);
        if (abs(leftHeight - rightHeight) > 1) {
            flag = true;
            return -1;
        }
        return max(leftHeight, rightHeight) + 1;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目在思考和coding上并没有太大难度，值得总结的地方是如何在递归中进行判断，这里我采用的是多用一个变量记录的方法。这道题目的分享到这里，谢谢！
