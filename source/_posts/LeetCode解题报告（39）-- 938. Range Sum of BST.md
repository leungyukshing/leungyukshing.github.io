---
title: LeetCode解题报告（39）-- 938. Range Sum of BST
tags:
  - LeetCode
abbrlink: 3291
date: 2018-11-26 09:55:57
---
## Problem
Given the `root` node of a binary search tree, return the sum of values of all nodes with value between `L` and `R` (inclusive).

The binary search tree is guaranteed to have unique values.
<!-- more -->

**Example 1:**
```
Input: root = [10,5,15,3,7,null,18], L = 7, R = 15
Output: 32
```

**Example 2:**
```
Input: root = [10,5,15,3,7,13,18,1,null,6], L = 6, R = 10
Output: 23
```

**Note:**
  + The number of nodes in the tree is at most `10000`.
  + The final answer is guaranteed to be less than `2^31`.


## Analysis
&emsp;&emsp;这道题是关于二叉树的题目，以往也介绍过，可以使用递归的方法来解决。问题要求我们求的所有节点的值的和，并且这些节点的值在给定的范围内。使用递归的思想，我们只需要从每一个节点的角度出发去计算就可以了。如果当前节点的值在给定的范围内，那么就加上；否则，就不加自己的值。每个节点递归求解它的左子树和右子树。

## Solution
&emsp;&emsp;按照分析的思路写代码就可以了，注意判断NULL的情况。

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
    int rangeSumBST(TreeNode* root, int L, int R) {
        if (root == NULL) {
            return 0;
        }
        int left = rangeSumBST(root->left, L, R);
        int right = rangeSumBST(root->right, L, R);
        if (root->val <= R && root->val >= L) {
            return left + right + root->val;
        }
        else {
            return left + right;
        }
    }

};
```
**运行时间：**约68ms，超过93.1%的CPP代码。

## Summary
&emsp;&emsp;这道题是二叉树问题的一个变种，在求整棵树的值的基础上加上了一个区间限制，但整体的思路还是不变的。把握好这点，只需要在返回结果的时候加上判断即可。这道题的分析到这里，谢谢！
