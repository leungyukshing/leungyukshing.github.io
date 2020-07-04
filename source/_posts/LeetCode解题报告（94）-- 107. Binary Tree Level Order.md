---
title: LeetCode解题报告（94）-- 107. Binary Tree Level Order Traversal II
tags:
  - LeetCode
date: 2020-07-04 13:52:30
mathjax: true


---

## Problem

Given a binary tree, return the *bottom-up level order* traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

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

return its bottom-up level order traversal as:

```
[
  [15,7],
  [9,20],
  [3]
]
```

------

## Analysis

&emsp;&emsp;二叉树遍历的经典题目之一——层次遍历，需要按照每一层的顺序来了遍历二叉树。二叉树的遍历无非就两种方法，递归和用队列，在层次遍历的时候使用队列会更加容易理解。在遍历某一层的时候，我们把下一层的节点都放到队列中，然后把这一层遍历过的节点的值存放起来就可以了。

------

## Solution

&emsp;&emsp;思路是非常简单的，实现起来有几个细节需要注意。一个是区分两个层，因为所有的节点都是扔进同一个队列的，所以要在这个队列中区分出什么时候要到下一个层。一个很巧妙的方法就是在一开始遍历的时候就先读取一遍队列的size，然后只拿出`size`个节点，那么之后入队的节点就不会被拿到，就被认为是下一层的节点了。这样就能够很好地区分两个层。然后每一层遍历后的值使用一个vector记录下来，最后返回结果就可以了。

&emsp;&emsp;特别注意跳过空节点，空节点不需要放入队列中遍历。

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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int> > result;
        if (!root) {
            return result;
        }
        
        queue<TreeNode*> q;
        q.push(root);
        while (!q.empty()) {
            vector<int> oneLevel;
            for (int i = q.size(); i > 0; i--) {
                TreeNode* node = q.front();
                q.pop();
                oneLevel.push_back(node->val);
                if (node->left) {
                    q.push(node->left);                
                }
                if (node->right) {
                    q.push(node->right);
                }
            }
            result.insert(result.begin(), oneLevel);
        }
        return result;
    }
};
```

------

## Summary

&emsp;&emsp;这道题是非常典型的二叉树遍历题目，这里我用了队列+循环的方法做，当然使用递归也是可以完成的，可以加多一个参数去表示在第几层，这里就没有详细分析了。希望这篇博客能够帮助到您，感谢您的支持，欢迎转发、分享、评论，谢谢！
