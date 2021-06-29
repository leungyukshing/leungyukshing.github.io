---
title: LeetCode解题报告（120）-- 450. Delete Node in a BST
tags:
  - LeetCode
mathjax: true
abbrlink: 57400
date: 2020-08-31 19:05:12
---

## Problem

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

1. Search for a node to remove.
2. If the node is found, delete the node.

**Note:** Time complexity should be O(height of tree).

<!-- more -->

**Example:**

```
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

Given key to delete is 3. So we find the node with value 3 and delete it.

One valid answer is [5,4,6,2,null,null,7], shown in the following BST.

    5
   / \
  4   6
 /     \
2       7

Another valid answer is [5,2,6,null,4,null,7].

    5
   / \
  2   6
   \   \
    4   7
```

------

## Analysis

&emsp;&emsp;经典的二叉树题目，在搜索二叉树中删除某个指定的值的节点。算法很简单，就是遍历找到这个节点，然后删掉。遍历的过程比较简单，通过递归就很容易实现了。删除的情况想起来比较复杂，这里直接就按照被删除节点分情况给出了：

1. 叶子节点：因为没有左右子树，直接删除即可；
2. 有一颗子树：无论是左子树还是右子树，用下一个节点替代自己即可。
3. 有两颗子树：这种情况是比较复杂的，我们既可以找左子树中最大的元素去替代要被删除的节点，也可以找右子树中最小的元素去替代要被删除的节点。通常的做法都是**找右子树中最小的元素**。

------

## Solution

&emsp;&emsp;上述的三种情况前两种比较好写，主要是第三种，找右子树中最小的元素。实际上是找到右子树中的最左子树，该元素就是最小的，然后把它的值和要被删除的节点的值交换（实际上不交换也可以，直接赋值），最后还是调用递归把这个节点删掉。

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
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root) {
            return NULL;
        }
        if (root->val > key) {
            root->left = deleteNode(root->left, key);
        } else if (root->val < key) {
            root->right = deleteNode(root->right, key);
        } else {
            if (!root->left || !root->right) {
                root = (root->left) ? root->left: root->right;
            } else if (!root->left && !root->right) {
                delete root;
                root = NULL;
            } else {
                TreeNode* current = root->right;
                while (current->left) {
                    current = current->left;
                }
                root->val = current->val;
                root->right = deleteNode(root->right, root->val);
            }
        }
        return root;
    }
};
```

------

## Summary

&emsp;&emsp;这道题非常经典，总结好三种情况的话coding起来就非常方便了。这道题的分析到这里，谢谢！
