---
title: LeetCode解题报告（375) -- 114. Flatten Binary Tree to Linked List
tags:
  - LeetCode
mathjax: true
abbrlink: 65352
date: 2021-05-17 09:42:41
---

## Problem

Given the `root` of a binary tree, flatten the tree into a "linked list":

- The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
- The "linked list" should be in the same order as a [**pre-order** **traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR) of the binary tree.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

```
Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Example 3:**

```
Input: root = [0]
Output: [0]
```



**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-100 <= Node.val <= 100`

 

**Follow up:** Can you flatten the tree in-place (with `O(1)` extra space)?

------

## Analysis

&emsp;&emsp;题目要求把一个二叉树打平成一条链表，顺序按照前序遍历的结果。最简单的做法就是先前序遍历一遍，把结果存到一个数组中，最后再从数组中重新构建一条链表。但是题目的Follow up要求的时候不使用额外的空间，意味着要通过操作二叉树的节点来打平成链表。

&emsp;&emsp;首先，对于某一个节点来说，前序遍历的顺序是自己->左子树->右子树，所以如果要打平成链表的话，就要把左子树移动到右子树之前。我们首先要找到最左的叶子结点，然后，把这个节点作为其父节点的右节点，原来的右节点作为这个新右节点的右节点。对于二叉树的操作递归进行即可。

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
    void flatten(TreeNode* root) {
        if (!root) return;
        if (root->left) flatten(root->left);
        if (root->right) flatten(root->right);
        TreeNode *tmp = root->right;
        root->right = root->left;
        root->left = NULL;
        while (root->right) root = root->right;
        root->right = tmp;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是二叉树题目中比较经典的题目。难度中等，主要还是考察前序遍历的特点。处理方法上二叉树问题主要还是使用递归解决。这道题目的分享到这里，感谢你的支持！
