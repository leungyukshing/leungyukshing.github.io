---
title: LeetCode解题报告（244）-- 98. Validate Binary Search Tree
tags:
  - LeetCode
mathjax: true
date: 2020-12-16 20:50:30

---

## Problem

Given the `root` of a binary tree, *determine if it is a valid binary search tree (BST)*.

A **valid BST** is defined as follows:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

<!-- more -->

**Example 1:**

![Example 1](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

```
Input: root = [2,1,3]
Output: true
```

**Example 2:**

![Example 2](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)

```
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-231 <= Node.val <= 231 - 1`

------

## Analysis

&emsp;&emsp;题目给定一颗二叉树，要求判断是否一颗BST。**BST最重要的性质就是其中序遍历的结果是有序的**。所以我利用了这个特点，如果是valid的BST，那么中序遍历的结果就是有序的；如果是invalid的BST，那么中序遍历的结果就是无序的。

------

## Solution

&emsp;&emsp;把二叉树中序遍历的结果先存到一个数组中，然后检查数组是否有序即可。

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
    bool isValidBST(TreeNode* root) {
        vector<int> arr;
        helper(root, arr);
        int size = arr.size();
        for (int i = 0; i < size - 1; i++) {
            if (arr[i] >= arr[i + 1]) {
                return false;
            }
        }
        return true;
    }
private:
    void helper(TreeNode* root, vector<int>& arr) {
        if (!root) {
            return;
        }
        helper(root->left, arr);
        arr.push_back(root->val);
        helper(root->right, arr);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目表面上是验证BST，实际上考察的是BST的中序遍历。如果对BST这些性质比较熟悉的话，就能很容易地pass这道题目。这道题目的分享到这里，谢谢您的支持！
