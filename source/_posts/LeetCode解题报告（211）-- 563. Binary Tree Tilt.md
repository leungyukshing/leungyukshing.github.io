---
title: LeetCode解题报告（211）-- 563. Binary Tree Tilt
tags:
  - LeetCode
mathjax: true
date: 2020-11-08 22:33:21


---

## Problem

Given the `root` of a binary tree, return *the sum of every tree node's **tilt**.*

The **tilt** of a tree node is the **absolute difference** between the sum of all left subtree node **values** and all right subtree node **values**. If a node does not have a left child, then the sum of the left subtree node **values** is treated as `0`. The rule is similar if there the node does not have a right child.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/10/20/tilt1.jpg)

```
Input: root = [1,2,3]
Output: 1
Explanation: 
Tilt of node 2 : |0-0| = 0 (no children)
Tilt of node 3 : |0-0| = 0 (no children)
Tile of node 1 : |2-3| = 1 (left subtree is just left child, so sum is 2; right subtree is just right child, so sum is 3)
Sum of every tilt : 0 + 0 + 1 = 1
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/10/20/tilt2.jpg)

```
Input: root = [4,2,9,3,5,null,7]
Output: 15
Explanation: 
Tilt of node 3 : |0-0| = 0 (no children)
Tilt of node 5 : |0-0| = 0 (no children)
Tilt of node 7 : |0-0| = 0 (no children)
Tilt of node 2 : |3-5| = 2 (left subtree is just left child, so sum is 3; right subtree is just right child, so sum is 5)
Tilt of node 9 : |0-7| = 7 (no left child, so sum is 0; right subtree is just right child, so sum is 7)
Tilt of node 4 : |(3+5+2)-(9+7)| = |10-16| = 6 (left subtree values are 3, 5, and 2, which sums to 10; right subtree values are 9 and 7, which sums to 16)
Sum of every tilt : 0 + 0 + 0 + 2 + 7 + 6 = 15
```

**Example 3:**

![Example3](https://assets.leetcode.com/uploads/2020/10/20/tilt3.jpg)

```
Input: root = [21,7,14,1,1,2,2,3,3]
Output: 9
```

------

## Analysis

&emsp;&emsp;这道二叉树相关的题目要求计算二叉树的`tilt`值。定义每个root的`tilt`值为**左子树的和与右子树的和之差的绝对值**。所以要计算这个`tilt`值就要知道左子树和右子树的和，这个并不困难，用递归就能计算出来。因为是计算整棵树每个节点的`tilt`的和，而不是只是根节点的`tilt`的值，所以我们用一个变量在递归的过程累加起来即可。

------

## Solution

&emsp;&emsp;简单的二叉树递归遍历。

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
    int findTilt(TreeNode* root) {
        int result = 0;
        helper(root, result);
        return result;
    }
private:
    int helper(TreeNode* root, int& result) {
        if (!root) {
            return 0;
        }
        int leftSum = helper(root->left, result);
        int rightSum = helper(root->right, result);
        result += abs(leftSum - rightSum);
        return root->val + leftSum + rightSum;
    }
};
```

------

## Summary

&emsp;&emsp;虽然这道题目的背景比较新，并不是递归直接作为答案，而是通过递归计算出中间值，相加而成答案，但是本质还是对二叉树的遍历，把握住这个知识点解决这道题目就很简单了。这道题目的分享到这里，谢谢！
