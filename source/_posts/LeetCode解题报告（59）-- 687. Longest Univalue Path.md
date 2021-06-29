---
title: LeetCode解题报告（59）-- 687. Longest Univalue Path
tags:
  - LeetCode
abbrlink: 57359
date: 2019-09-25 23:54:03
---

## Problem

Given a binary tree, find the length of the longest path where each node in the path has the same value. This path may or may not pass through the root.

The length of path between two nodes is represented by the number of edges between them.

<!-- more -->

**Example 1:**

```
Input:
              5
             / \
            4   5
           / \   \
          1   1   5
Output: 2
```

**Example 2:**

```
Input:
              1
             / \
            4   5
           / \   \
          4   4   5
Output: 2
```

**Note:** The given binary tree has not more than 10000 nodes. The height of the tree is not more than 1000.

------

## Analysis

&emsp;&emsp;这道题是二叉树的题目，要求我们在一棵二叉树中，找到一条**最长的**、**由值相等的节点**组成的路径。这道题首先要明确：首先这条路径是不一定经过根节点的（详见Example2），所以这也说明我们很难单纯地使用递归的方法自上而下地来求出答案。然后我们看递归过程中我们要返回的是什么东西，这里应该是定义返回的是：**以该节点为起点所能获得的最长的路径长度**。这里可能会有疑问，为什么不是直接返回**以该节点为root的子树的最长的路径长度**呢？

&emsp;&emsp;实际上，我们考虑这样一种情况，我们可以由左子树到root再到右子树组成一条最长路径，但是递归上去的时候就只能取一个子树，所以没有办法通过一个返回值来包含这两种信息。因此我额外使用多一个变量来保存当前能找到的最优的路径即可。

------

## Solution

&emsp;&emsp;递归的时候判断root与左右节点是否相同，如果相同则路径长度+1，然后每次迭代维护最长的路径长度。而每个root的最长路径总是由其左子树最长路径加上右子树最场路径得到的。

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
    int longestUnivaluePath(TreeNode* root) {
        result = 0;
        int temp = recursive(root);
        return result;
    }
private:
    int result;
    int recursive(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }
        
        int left = 0;
        int right = 0;
        
        left = recursive(root->left);
        right = recursive(root->right);
        
        if (root->left != NULL && root->val == root->left->val) {
            left++;
        }
        else {
            left = 0;
        }
        
        if (root->right != NULL && root->val == root->right->val) {
            right++;
        }
        else {
            right = 0;
        }
        
        result = max(result, left + right);
        return max(left, right);
    }
};
```

------

## Summary

&emsp;&emsp;这是一道有关二叉树的题目，虽然题目难度是easy，但是实际上做起来却不算简单。难点是理解多种case，比如某个root的最长路径是左子树加右子树。使用多一个变量来维护全剧最优也是解决这道题的突破口。这道题目的分享到这里，感谢你的支持！
