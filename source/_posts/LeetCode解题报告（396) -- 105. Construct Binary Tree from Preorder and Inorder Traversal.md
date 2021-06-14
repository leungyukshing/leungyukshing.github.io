---
title: LeetCode解题报告（396) -- 105. Construct Binary Tree from Preorder and Inorder Traversal
tags:
  - LeetCode
mathjax: true
date: 2021-06-14 16:22:11

---

## Problem

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return *the binary tree*.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```



**Constraints:**

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- `-3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` and `inorder` consist of **unique** values.
- Each value of `inorder` also appears in `preorder`.
- `preorder` is **guaranteed** to be the preorder traversal of the tree.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.

------

## Analysis

&emsp;&emsp;标准的根据前序和中序遍历恢复二叉树题目。前序提供了根节点的位置，中序提供了左右子树的位置。我们先根据前序的第一个元素，找到在中序对应的位置`i`，在中序数组中，`[iLeft, i]`是左子树的长度，`[i + 1, iRight]`是右子树的长度。然后继续递归下去即可。

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
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return helper(preorder, 0, preorder.size() - 1, inorder, 0, inorder.size() - 1);
    }
private:
    TreeNode* helper(vector<int>& preorder, int pLeft, int pRight, vector<int>& inorder, int iLeft, int iRight) {
        if (pLeft > pRight || iLeft > iRight) {
            return NULL;
        }
        
        // preorder[pLeft] is root
        int i = 0;
        for (i = iLeft; i <= iRight; i++) {
            if (preorder[pLeft] == inorder[i]) {
                break;
            }
        }
        TreeNode* node = new TreeNode(preorder[pLeft]);
        node->left = helper(preorder, pLeft + 1, pLeft + i - iLeft, inorder, iLeft, i - 1);
        node->right = helper(preorder, pLeft + i - iLeft + 1, pRight,inorder, i + 1, iRight);
        return node;
    }
};
```

------

## Summary

&emsp;&emsp;这道题比较简单，有一个基本的模式和套路，只需要找到根节点和区间，就能重构二叉树。这道题目的分享到这里，感谢你的支持！
