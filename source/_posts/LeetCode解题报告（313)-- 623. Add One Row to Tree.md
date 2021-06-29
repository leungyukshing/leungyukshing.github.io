---
title: LeetCode解题报告（313)-- 623. Add One Row to Tree
tags:
  - LeetCode
mathjax: true
abbrlink: 44621
date: 2021-03-10 01:49:11
---

## Problem

Given the root of a binary tree, then value `v` and depth `d`, you need to add a row of nodes with value `v` at the given depth `d`. The root node is at depth 1.

The adding rule is: given a positive integer depth `d`, for each NOT null tree nodes `N` in depth `d-1`, create two tree nodes with value `v` as `N's` left subtree root and right subtree root. And `N's` **original left subtree** should be the left subtree of the new left subtree root, its **original right subtree** should be the right subtree of the new right subtree root. If depth `d` is 1 that means there is no depth d-1 at all, then create a tree node with value **v** as the new root of the whole original tree, and the original tree is the new root's left subtree.

<!-- more -->

**Example 1:**

```
Input: 
A binary tree as following:
       4
     /   \
    2     6
   / \   / 
  3   1 5   

v = 1

d = 2

Output: 
       4
      / \
     1   1
    /     \
   2       6
  / \     / 
 3   1   5   

```

**Example 2:**

```
Input: 
A binary tree as following:
      4
     /   
    2    
   / \   
  3   1    

v = 1

d = 3

Output: 
      4
     /   
    2
   / \    
  1   1
 /     \  
3       1
```

**Note:**

1. The given d is in range [1, maximum depth of the given tree + 1].
2. The given binary tree has at least one tree node.

------

## Analysis

&emsp;&emsp;题目给出一颗二叉树，要求在指定的层插入指定的值，插入的方式是，在这一层之下插入指定的值，然后原来的左子树，成为新的左节点的左子树，原来的右子树，成为新的右节点的右子树。

&emsp;&emsp;理解好题意后，这就是基于层次遍历做了，首先找到对应的层，然后对每一个节点都插入即可，插入的时候要遵照题意，处理好左、右子树的关系即可。

------

## Solution

&emsp;&emsp;这里有个特殊情况需要考虑，如果在第一层插入，即替换掉根节点的情况，此时原来的root就成了左子树，需要在层次遍历前特殊处理。

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
    TreeNode* addOneRow(TreeNode* root, int v, int d) {
        if (!root) {
            return root;
        }
        if (d == 1) {
            TreeNode* newRoot = new TreeNode(v);
            newRoot->left = root;
            return newRoot;
        }
        
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            if (--d == 0) {
                return root;
            }
            int size = q.size();
            for (int i = 0; i < size; i++) {
                auto node = q.front();
                q.pop();
                if (d == 1) {
                    TreeNode* left = node->left;
                    TreeNode* right = node->right;
                    node->left = new TreeNode(v);
                    node->right = new TreeNode(v);
                    node->left->left = left;
                    node->right->right = right;
                } else {
                    if (node->left){
                        q.push(node->left);
                    }
                    if (node->right) {
                        q.push(node->right);
                    }
                }
            }
        }
        return root;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目还是基于层次遍历的二叉树题目，增加了插入的操作。难度中等，主要理解好插入后的左右子树怎么摆放即可。这道题目的分享到这里，感谢你的支持！
