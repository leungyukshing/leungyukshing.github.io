---
title: LeetCode解题报告（234）-- 897. Increasing Order Search Tree
tags:
  - LeetCode
mathjax: true
date: 2020-12-04 10:23:11

---

## Problem

Given the `root` of a binary search tree, rearrange the tree in **in-order** so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only one right child.

<!-- more -->

**Example 1:**

![Example 1](https://assets.leetcode.com/uploads/2020/11/17/ex1.jpg)

```
Input: root = [5,3,6,2,4,null,8,1,null,null,null,7,9]
Output: [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]
```

**Example 2:**
![Example 2](https://assets.leetcode.com/uploads/2020/11/17/ex2.jpg)

```
Input: root = [5,1,7]
Output: [1,null,5,null,7]
```

**Constraints:**

- The number of nodes in the given tree will be in the range `[1, 100]`.
- `0 <= Node.val <= 1000`

------

## Analysis

&emsp;&emsp;这道题目的意思不难，但是操作起来有点绕。题目给出一颗BST，要求我们重构这颗二叉树，使得所有的节点都没有左子树。这道题目实际上有两个要求，一个是排序，一个是只能有右子树。第一个要求比较好满足，因为对于BST来说，中序遍历就是有序的。我们把重点放在第二个要求，对于某个节点`node`来说，这里要调整的是`node`和它的左子树`node->left`之间的指针关系。因为在进行左子树递归的时候就需要调整好这个关系，所以在递归的时候需要把父节点带上，这样才能改变`node->left`和`node`的关系。左子树的递归完成后，需要取消`node->left`，因为此时的`node `已经不再是新的树根，它已经被自己的左子树包含了。右子树的处理就比较简单，不需要考虑指针关系。

------

## Solution

&emsp;&emsp;特别注意左子树处理完之后要`root->left`。

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
    TreeNode* increasingBST(TreeNode* root) {
        return helper(root, NULL);
    }
private:
    TreeNode* helper(TreeNode* root, TreeNode* pre) {
        if (!root) {
            return pre;
        }
        TreeNode* left = helper(root->left, root);
        root->left = NULL;
        root->right = helper(root->right, pre);
        return left;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是二叉树题目的一个小变种，其本质还是中序遍历，只不过增加了一些结构上的要求。这里的关键还是在递归的时候传入父节点`pre`，使得左子树能够指向父节点。这道题目的分享到这里，谢谢您的支持！
