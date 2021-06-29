---
title: LeetCode解题报告（66）-- 1038. Binary Search Tree to Greater Sum Tree
tags:
  - LeetCode
abbrlink: 53036
date: 2019-09-30 18:26:40
---

## Problem


Given the root of a binary **search** tree with distinct values, modify it so that every `node` has a new value equal to the sum of the values of the original tree that are greater than or equal to `node.val`.

As a reminder, a *binary search tree* is a tree that satisfies these constraints:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

<!-- more -->

**Example 1:**

**![img](https://assets.leetcode.com/uploads/2019/05/02/tree.png)**

```
Input: [1,2,3,1]
Input: [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
Output: [30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```

**Note:**

1. The number of nodes in the tree is between `1` and `100`.
2. Each node will have value between `0` and `100`.
3. The given tree is a binary search tree.

------

## Analysis

&emsp;&emsp;这道题是二叉树中比较少见的题目，题目的要求可能有些难理解，它要求我们将每个节点的值设置为：**比该节点值大的所有节点的值的和**。实际上我们可以把问题转化为**找到比该节点大的所有节点**。比某个节点大的节点有以下几种情况：

- 若以该节点为根节点，比它大的节点就是它的右子树；
- 若该节点在左子树，比它大的节点就是它的父节点加上它的右子树；
- 若该节点在右子树，比它大的节点就是它的右子树。

&emsp;&emsp;从上面看第二点好像有点困难，因为我们不希望在coding的过程中需要从一个节点访问它的父节点，我们可以换个角度来看。假设我们从一个根节点出发，那么比它大的节点就是它的**右子树**，那么这个根节点的值就应该是它的右子树节点的和。然后对于它的左子树来说，实际上只需要加上自己的值就能够达到题目的要求。

&emsp;&emsp;因此这里的计算顺序很重要，要先算右子树，然后算自己的值，再算左子树的值。

------

## Solution

&emsp;&emsp;二叉树的题目还是基于递归来做，从上一part的分析不难看出，我们求解的顺序有点像后序遍历，这里我额外增加了一个变量来存放需要累加的和。

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
    TreeNode* bstToGst(TreeNode* root) {
        if (root == NULL)
            return NULL;
        int sum = 0;
        recursive(root, sum);
        return root;
    }
    
private:
    void recursive(TreeNode* root, int& sum) {
        if (root == NULL) {
            return;
        }
        
        recursive(root->right, sum);
        root->val += sum;
        sum = root->val;
        recursivce(root->left, sum);
    }
};
```

------

## Summary

&emsp;&emsp;这道题是加了点变化的二叉树题目，其本质的思想就是后序遍历。难点在于把题目的要求提炼成对树遍历的顺序，把握好先后顺序就能很轻易地解决这道题目。欢迎大家转发、评论，谢谢！

