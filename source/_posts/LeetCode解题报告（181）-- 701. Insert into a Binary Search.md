---
title: LeetCode解题报告（181）-- 701. Insert into a Binary Search Tree
tags:
  - LeetCode
mathjax: true
date: 2020-10-06 20:41:03


---

## Problem

You are given the `root` node of a binary search tree (BST) and a `value` to insert into the tree. Return *the root node of the BST after the insertion*. It is **guaranteed** that the new value does not exist in the original BST.

**Notice** that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return **any of them**.

<!-- more -->

**Example 1:**

```
Input: root = [4,2,7,1,3], val = 5
Output: [4,2,7,1,3,5]
```

![Example 1](https://assets.leetcode.com/uploads/2020/10/05/insertbst.jpg)

**Example 2:**

```
Input: root = [40,20,60,10,30,50,70], val = 25
Output: [40,20,60,10,30,50,70,null,null,25]
```

**Example 3:**

```
Input: root = [4,2,7,1,3,null,null,null,null,null,null], val = 5
Output: [4,2,7,1,3,5]
```

**Constraints:**

- The number of nodes in the tree will be in the range `[0, 104]`.
- `-108 <= Node.val <= 108`
- All the values `Node.val` are **unique**.
- `-108 <= val <= 108`
- It's **guaranteed** that `val` does not exist in the original BST.

------

## Analysis

&emsp;&emsp;这道题目是二叉树中非常经典的插入题目，在一刻BST中插入元素。思路其实很简单，通过和root比对，递归地去左子树或者右子树寻找，找到之后，新增加一个节点即可。

------

## Solution

&emsp;&emsp;因为新增加的节点需要让父节点指向，所以在父节点的时候就进行判断，如果小于root的值且左子树为空，则插入；或者大于root的值且右子树为空，也是插入操作。

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
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (!root) {
            root = new TreeNode(val);
            return root;
        }
        helper(root, val);
        return root;
    }
private:
    void helper(TreeNode* root, int val) {
        if (val < root->val) {
            if (root->left != NULL) {
                helper(root->left, val);
            } else {
                root->left = new TreeNode(val);
                return;
            }
        } else {
            if (root->right != NULL) {
                helper(root->right, val);
            } else {
                root->right = new TreeNode(val);
                return;
            }
        }
    }
};
```

------

## Summary

&emsp;&emsp;这道题目对于熟悉二叉树的朋友来说应该是easy的，在这里记录分享一下。这道题的分析到这里，谢谢！
