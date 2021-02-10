---
title: LeetCode解题报告（290）-- 538. Convert BST to Greater Tree
tags:
  - LeetCode
mathjax: true
date: 2021-02-10 03:02:28

---

## Problem

Given the `root` of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

As a reminder, a *binary search tree* is a tree that satisfies these constraints:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Note:** This question is the same as 1038: https://leetcode.com/problems/binary-search-tree-to-greater-sum-tree/

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2019/05/02/tree.png)

```
Input: root = [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
Output: [30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```

**Example 2:**

```
Input: root = [0,null,1]
Output: [1,null,1]
```

**Example 3:**

```
Input: root = [1,0,2]
Output: [3,3,2]
```

**Example 4:**

```
Input: root = [3,2,4,1]
Output: [7,9,4,10]
```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-104 <= Node.val <= 104`
- All the values in the tree are **unique**.
- `root` is guaranteed to be a valid binary search tree.

------

## Analysis

&emsp;&emsp;题目给出一棵BST，要求把每个节点的值都改为原来的值加上整棵BST中比这个节点大的值。这里的难点就在于如何找到所有比这个节点大的节点值之和，实际上BST的中序遍历能给我们提供从小到大的排序，而只要我们改变一下左右子树遍历的顺序，就能够变成从大到小的顺序，所以我们只需要进行中序遍历，先右子树，然后是root本身，最后才到左子树，维护一个`sum`记录比当前节点大的值之和，递归求解即可。

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
    TreeNode* convertBST(TreeNode* root) {
        if (!root) {
            return NULL;
        }
        convertBST(root->right);
        root->val += sum;
        sum = root->val;
        convertBST(root->left);
        return root;
    }
private:
    int sum = 0;
};
```

------

## Summary

&emsp;&emsp;这又是一道二叉树遍历的变种题，其基础是二叉树的中序遍历。这里我们利用了中序遍历得到有序数组的性质进行求解。这道题目的分享到这里，谢谢您的支持！
