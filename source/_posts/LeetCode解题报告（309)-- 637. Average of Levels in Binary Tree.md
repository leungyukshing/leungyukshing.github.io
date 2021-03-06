---
title: LeetCode解题报告（309)-- 637. Average of Levels in Binary Tree
tags:
  - LeetCode
mathjax: true
date: 2021-03-06 16:27:52

---

## Problem

Given a non-empty binary tree, return the average value of the nodes on each level in the form of an array.

<!-- more -->

**Example 1:**

```
Input:
    3
   / \
  9  20
    /  \
   15   7
Output: [3, 14.5, 11]
Explanation:
The average value of nodes on level 0 is 3,  on level 1 is 14.5, and on level 2 is 11. Hence return [3, 14.5, 11].
```

**Note:**

1. The range of node's value is in the range of 32-bit signed integer.

------

## Analysis

&emsp;&emsp;这道题目要求计算二叉树中每一层的平均值，然后以数组的形式返回。和二叉树层级有关的题目，很明显就是层次遍历了，计算出每一层的总和，然后求出平均值即可。

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
    vector<double> averageOfLevels(TreeNode* root) {
        queue<TreeNode*> q;
        vector<double> result;
        if (!root) {
            return result;
        }
        q.push(root);
        
        while (!q.empty()) {
            int size = q.size();
            long long sum = 0;
            for (int i = 0; i < size; i++) {
                TreeNode *node = q.front();
                q.pop();
                if (node->left) {
                    q.push(node->left);
                }
                if (node->right) {
                    q.push(node->right);
                }
                sum += node->val;
            }
            double avg = (double)sum / size;
            result.push_back(avg);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目层次遍历的一个小变形，难度不大。这道题目的分享到这里，感谢你的支持！
