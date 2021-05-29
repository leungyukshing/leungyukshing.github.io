---
title: LeetCode解题报告（377) -- 968. Binary Tree Cameras
tags:
  - LeetCode
mathjax: true
date: 2021-05-29 12:27:58


---

## Problem

Given a binary tree, we install cameras on the nodes of the tree. 

Each camera at a node can monitor **its parent, itself, and its immediate children**.

Calculate the minimum number of cameras needed to monitor all nodes of the tree.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_01.png)

```
Input: [0,0,null,0,0]
Output: 1
Explanation: One camera is enough to monitor all nodes if placed as shown.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2018/12/29/bst_cameras_02.png)

```
Input: [0,0,null,0,null,0,null,null,0]
Output: 2
Explanation: At least two cameras are needed to monitor all nodes of the tree. The above image shows one of the valid configurations of camera placement.
```



**Note:**

1. The number of nodes in the given tree will be in the range `[1, 1000]`.
2. **Every** node has value 0.

------

## Analysis

&emsp;&emsp;这道题给出一颗二叉树，如果在某个节点放置了一个照相机，则它可以监控到**它本身**、**它的父节点**、**它的叶子节点**。要求我们求出最少的相机个数，能够监控到整颗二叉树。我们先从直观的角度看看相机应该放在什么地方比较好：

- 放在叶子节点：此时只能监控到叶子节点本身和它的父节点两个节点；
- 放在根节点：此时只能监控到根节点本身和它的两个子节点（如果有），一共三个；
- 放在叶子节点的父节点：此时可以监控到节点本身，和它的两个子节点（如果有），再加上它的父节点，一共四个。

&emsp;&emsp;所以最优的方法应该是第三种，接下来就是我们怎么找到第三种的节点。我们定义三种状态：

- 0:表示当前节点是叶子节点；
- 1:表示当前节点是叶子节点的父节点，并放置了相机；
- 2:表示当前节点是叶子节点的爷爷节点，并被相机监控了。

&emsp;&emsp;然后就是递归处理了。

- 如果当前节点不存在，返回2，因为不存在的节点也相当于被相机监控了；
- 如果当前节点的左右子树中，有一个返回了0，说明当前节点至少有一个子节点是叶子节点，所以当前节点需要放置相机，向上返回1；
- 如果当前节点的左右子树中，有一个返回了1，说明当前节点至少有一个子节点被放置了相机，所以当前节点也被监控了，向上返回2。

&emsp;&emsp;最后就需要处理根节点的情况，当最后的返回值是0时，说明根节点本身就是叶子节点，需要放置相机。

------

## Solution

&emsp;&emsp;无

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
    int minCameraCover(TreeNode* root) {
        int result = 0;
        return (helper(root, result) < 1 ? 1: 0) + result;
    }
private:
    int helper(TreeNode* node, int& result) {
        if (!node) {
            return 2;
        }
        int left = helper(node->left, result);
        int right = helper(node->right, result);
        
        if (left == 0 || right == 0) {
            result++;
            return 1;
        }
        return (left == 1 || right == 1) ? 2: 0;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较复杂的二叉树题目，基本的框架还是递归，难点在于引入了三个状态去区分出哪些位置需要放相机，哪些位置不需要放。这道题目的分享到这里，感谢你的支持！
