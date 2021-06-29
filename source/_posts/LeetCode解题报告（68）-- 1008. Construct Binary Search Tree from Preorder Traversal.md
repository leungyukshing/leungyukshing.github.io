---
title: LeetCode解题报告（68）-- 1008. Construct Binary Search Tree from Preorder Traversal
tags:
  - LeetCode
abbrlink: 10482
date: 2019-10-01 10:20:31
---

## Problem

Return the root node of a binary **search** tree that matches the given `preorder` traversal.

*(Recall that a binary search tree is a binary tree where for every node, any descendant of node.left has a value < node.val, and any descendant of node.right has a value > node.val.  Also recall that a preorder traversal displays the value of the node first, then traverses node.left, then traverses node.right.)*

<!-- more -->

**Example 1:**

```
Input: [8,5,1,7,10,12]
Output: [8,5,10,1,7,null,12]
```

![Binary Tree](https://assets.leetcode.com/uploads/2019/03/06/1266.png)

**Note**

1. `1 <= preorder.length <= 100`
2. The values of `preorder` are distinct.

------

## Analysis

&emsp;&emsp;这道题是很经典的二叉树构建问题。题目给出前序遍历的结果，要求我们重新构建二叉树。我们都知道，构建二叉树一般都是基于递归的，而递归的难点在于怎么区分好左子树和右子树。因此我们的思路可以转换为**在给定的数组中区分左右子树**。

&emsp;&emsp;先来看前序遍历的特点，首先是root本身，然后是左子树，最后是右子树。左子树的值都比root要小，而右子树的值都比root大，因此在一个数组中出现的规律应该是：**以某一个数`a[i]`为root，后面若干个数都比它小，是它的左子树，然后从一个值开始比它大，是它的右子树**。这样，只要我们找到这个比它大的值，就能够划分出左右子树。

------

## Solution

&emsp;&emsp;为了递归的方便，我们可以在若干个递归过程中均使用同一个vector，我们可以通过使用下标限定区间来确定某一棵树的范围。

------

## Code

```c++
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
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        return recursive(preorder, 0, preorder.size() - 1);
    }
    
private:
    TreeNode* recursive(vector<int>& preorder, int start, int end) {
        if (start > end) {
            return NULL;
        }
        TreeNode* root = new TreeNode(preorder[start]);
        int split = end + 1;
        for (int i = start; i <= end; i++) {
            if (preorder[i] > preorder[start]) {
                split = i;
                break;
            }
        }
        root->left = recursive(preorder, start + 1, split - 1);
        root->right = recursive(preorder, split, end);
        return root;
    }
};
```

------

## Summary

 &emsp;&emsp;题目难度是medium，但是这种题是属于二叉树的常规题目，建议大家还是要尽可能地熟悉不同遍历顺序建树的做法。这道题目的分享就到这里，谢谢您的支持！
