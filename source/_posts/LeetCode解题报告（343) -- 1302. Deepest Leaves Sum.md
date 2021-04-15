---
title: LeetCode解题报告（343) -- 1302. Deepest Leaves Sum
tags:
  - LeetCode
mathjax: true
date: 2021-04-16 01:17:22

---

## Problem

Given the `root` of a binary tree, return *the sum of values of its deepest leaves*.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2019/07/31/1483_ex1.png)

```
Input: root = [1,2,3,4,5,null,6,7,null,null,null,null,8]
Output: 15
```

**Example 2:**

```
Input: root = [6,7,8,2,7,1,3,9,null,1,4,null,null,null,5]
Output: 19
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `1 <= Node.val <= 100`

------

## Analysis

&emsp;&emsp;题目给出一颗二叉树，要求计算出**最深叶子节点的和**。这里的难点是要找到最深的，那么我们怎么判断是最深的呢？如果只遍历一次的话好像有点困难，那可以先遍历一次，找到最深的深度，第二次遍历的时候，当某个叶子节点的深度是最深深度的时候，就求和。

&emsp;&emsp;所以问题就拆分为两个子问题，第一个是求二叉树的深度，第二个是找到满足某个深度的节点。第一个问题很简单了，递归函数每次取左右子树的最大值然后加1返回；第二个问题的递归函数参数稍微多点，要把指定的深度传进去，还要把当前的深度传进去，最后还要把答案作为引用传进去。

------

## Solution

&emsp;&emsp;无。

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
    int deepestLeavesSum(TreeNode* root) {
        int maxDepth = helper(root);
        int result = 0;
        helper2(root, 1, maxDepth, result);
        return result;
    }
private:
    int helper(TreeNode* root) {
        if (!root) {
            return 0;
        }
        return max(helper(root->left), helper(root->right)) + 1;
    }
    void helper2(TreeNode* root, int level, int maxDepth, int &result) {
        if (!root) {
            return;
        }
        if (level == maxDepth) {
            result += root->val; 
        }
        helper2(root->left, level + 1, maxDepth, result);
        helper2(root->right, level + 1, maxDepth, result);
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是二叉树中比较简单的题目了，实际上是把两个小问题合并成一个大问题。二叉树的解法绝大多数还是基于递归，因此很多时候都会选择把答案以传引用的方式作为参数放到递归的函数中。这道题目的分享到这里，感谢你的支持！
