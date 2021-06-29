---
title: LeetCode解题报告（330) -- 971. Flip Binary Tree To Match Preorder Traversal
tags:
  - LeetCode
mathjax: true
abbrlink: 34974
date: 2021-04-04 15:07:52
---

## Problem

You are given the `root` of a binary tree with `n` nodes, where each node is uniquely assigned a value from `1` to `n`. You are also given a sequence of `n` values `voyage`, which is the **desired** [**pre-order traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order) of the binary tree.

<!-- more -->

Any node in the binary tree can be **flipped** by swapping its left and right subtrees. For example, flipping node 1 will have the following effect:

![img](https://assets.leetcode.com/uploads/2021/02/15/fliptree.jpg)

Return *a list of the values of all **flipped** nodes. You may return the answer in **any order**. If it is **impossible** to flip the nodes in the tree to make the pre-order traversal match* `voyage`*, return the list* `[-1]`.

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2019/01/02/1219-01.png)

```
Input: root = [1,2], voyage = [2,1]
Output: [-1]
Explanation: It is impossible to flip the nodes such that the pre-order traversal matches voyage.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2019/01/02/1219-02.png)

```
Input: root = [1,2,3], voyage = [1,3,2]
Output: [1]
Explanation: Flipping node 1 swaps nodes 2 and 3, so the pre-order traversal matches voyage.
```

**Example 3:**

![Example3](https://assets.leetcode.com/uploads/2019/01/02/1219-02.png)

```
Input: root = [1,2,3], voyage = [1,2,3]
Output: []
Explanation: The tree's pre-order traversal already matches voyage, so no nodes need to be flipped.
```

**Constraints:**

- The number of nodes in the tree is `n`.
- `n == voyage.length`
- `1 <= n <= 100`
- `1 <= Node.val, voyage[i] <= n`
- All the values in the tree are **unique**.
- All the values in `voyage` are **unique**.

------

## Analysis

&emsp;&emsp;题目给出一颗二叉树，还有一个前序遍历的数组，要求我们通过交换二叉树中的左右子树，使得二叉树的前序遍历结果满足题目给出的数组。如果不能通过交换操作，满足题目的要求，就返回-1作为答案。

&emsp;&emsp; 首先这道题目讨论的遍历顺序是前序遍历，所以在数组中只需要一直往后找即可，不用像中序遍历一样左右寻找，因此我们用一个下标表示当前匹配到哪个位置。因为是二叉树题目，还是使用递归解决。首先处理`root`，如果和遍历数组中的值已经对应不上，直接返回false，因为题目只允许交换左右子树，而根节点是不能动的；然后开始递归处理左右节点，如果左子节点的值和数组中的不一样，就交换左右子树，然后继续递归下去，如果值一样，则不需要操作继续递归下去即可。

------

## Solution

&emsp;&emsp;在递归过程中需要注意，因为整个过程都是用同一个前序遍历的数组，我们使用下标来定位到当前遍历到哪个位置，所以递归函数需要把这个下标也带上，同时应该是**传引用**，这样在递归完左子树后，右子树的下标才正确。

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
    vector<int> flipMatchVoyage(TreeNode* root, vector<int>& voyage) {
        vector<int> result;
        int i = 0;
        return helper(root, voyage, i, result) ? result: vector<int>{-1};
    }
private:
    bool helper(TreeNode* node, vector<int>& voyage, int &i, vector<int> &result) {
        if (!node) {
            return true;
        }
        if (node->val != voyage[i++]) {
            return false;
        }
        if (node->left && node->left->val != voyage[i]) {
            // left value is not equal to voyage, may need to flip
            result.push_back(node->val); // push root value into result
            return helper(node->right, voyage, i, result) && helper(node->left, voyage, i, result);
        }
        // left value is equal to voyage, no flip is needed, just check
        return helper(node->left, voyage, i, result) && helper(node->right, voyage, i, result);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目的分享到这里，感谢你的支持！
