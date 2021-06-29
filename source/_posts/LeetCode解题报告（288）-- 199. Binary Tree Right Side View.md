---
title: LeetCode解题报告（288）-- 199. Binary Tree Right Side View
tags:
  - LeetCode
mathjax: true
abbrlink: 46746
date: 2021-02-10 02:12:05
---

## Problem

Given a binary tree, imagine yourself standing on the *right* side of it, return the values of the nodes you can see ordered from top to bottom.

<!-- more -->

**Example:**

```
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

------

## Analysis

&emsp;&emsp;这是一道二叉树的遍历题，但是它只要求返回每一层最右边的元素。第一反应觉得好复杂，但是注意到是**每一层**，感觉可以往层次遍历上靠。因为在层次遍历的时候我们用的是队列，所以这个顺序性是可以保证的，所以我们只需要在层次遍历的基础上稍作改进。每一层遍历开始的时候，先把队尾的元素放到结果中，其他的不变。

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
    vector<int> rightSideView(TreeNode* root) {
        vector<int> result;
        queue<TreeNode*> q;
        if (!root) {
            return result;
        }
        q.push(root);
        while (!q.empty()) {
            TreeNode* temp = q.back();
            result.push_back(temp->val);
            int size = q.size();
            for (int i = 0; i < size; i++) {
                TreeNode* node = q.front();
                q.pop();
                if (node->left) {
                    q.push(node->left);
                }
                if (node->right) {
                    q.push(node->right);
                }
            }
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是二叉树遍历的一个变种，其基础是二叉树的层次遍历。所以对于二叉树来说，掌握几种基本的遍历方法对于求解更加复杂的题目是非常有帮助的。这道题目的分享到这里，谢谢您的支持！
