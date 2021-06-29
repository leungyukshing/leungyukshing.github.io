---
title: LeetCode解题报告（106）-- 103. Binary Tree Zigzag Level Order Traversal
tags:
  - LeetCode
mathjax: true
abbrlink: 32332
date: 2020-07-23 23:10:11
---

## Problem

Given a binary tree, return the *zigzag level order* traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

<!-- more -->

For example:
Given binary tree `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

return its zigzag level order traversal as:

```
[
  [3],
  [20,9],
  [15,7]
]
```

------

## Analysis

&emsp;&emsp;这道题目是二叉树遍历的题目，但是要求和之前有一些不一样，要求按照**Z**字型来遍历二叉树。第一下看好像无从下手，但是仔细分析可以看出来，实际上这就是一个**层次遍历**啊。只不过要根据层数，选择是从左到右，还是从右到左。

------

## Solution

&emsp;&emsp;层次遍历的做法还是保持不变，使用多一个变量`level`去记录当前是第几层，如果是偶数层的话，就需要把当前层遍历的结果reverse一下。

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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<int> row; // to store one level values
        vector<vector<int>> v;
        queue<TreeNode *> q;
        if (root == NULL) {
            return v;            
        }
        q.push(root);
        
        TreeNode *temp;
        int lev = 0; // level number
        while(!q.empty()) {
            int size = q.size(); // only traverse nodes in one level
            while(size--) {
                temp = q.front();
                q.pop();
                row.push_back(temp->val);
                if(temp->left != NULL) {
                    q.push(temp->left);
                }
                if(temp->right != NULL) {
                    q.push(temp->right);
                }
            }
            if(lev % 2) {
                // reverse according to level number
                int n = row.size();
                for(int i = 0; i < n/2; i++) {
                    swap(row[i], row[n-i-1]);
                }
            }
            v.push_back(row);
            lev++;
            row.clear();
        }
        return v;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目本质还是二叉树的层次遍历，只不过加入了一些额外的条件，是需要在层次遍历后特殊处理一下就好了。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
