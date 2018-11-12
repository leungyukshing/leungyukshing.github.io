---
title: LeetCode解题报告（25）-- 654. Maximum Binary Tree
tags:
  - LeetCode
abbrlink: 1441
date: 2018-10-30 00:08:47
---
## Problem
Given an integer array with no duplicates. A maximum tree building on this array is defined as follow:
  1. The root is the maximum number in the array.
  2. The left subtree is the maximum tree constructed from left part subarray divided by the maximum number.
  3. The right subtree is the maximum tree constructed from right part subarray divided by the maximum number.

Construct the maximum tree by the given array and output the root node of this tree.
<!-- more -->

**Example 1**:
```
Input: [3,2,1,6,0,5]
Output: return the tree root node representing the following tree:

      6
    /   \
   3     5
    \    /
     2  0
       \
        1
```

**Note:**
  1. The size of the given array will be in the range [1,1000].

## Analysis
&emsp;&emsp;这道题是关于由数组构建二叉树的。要求是使用当前数组最大的值作为根节点，在左边的值归到左子树，在右边的值归到右子树。之前也提到过，关于二叉树我们经常使用递归来解决问题。于是我们需要在数组中找出最大的元素，将数组一分为二，左半边是小于这个最大值的，右半边是大于这个最大值的。然后分别递归，构建当前节点的左子树和右子树即可。

## Solution
&emsp;&mesp;遍历一遍数组即可找到最大值，记录这个最大值用于构建当前节点，记录最大值对应的下标用于分割数组。左半边数组为`vector<int> left = vector<int>(nums.begin(), nums.begin() + maxIndex);`，右半边数组为`vector<int> right = vector<int>(nums.begin() + maxIndex + 1, nums.end());`，分别递归生成当前的左子树和右子树就可以了。

## Code
```C++
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
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        if (nums.empty())
            return NULL;
        int maxIndex = 0;
        int maxEle = -1;

        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] > maxEle) {
                maxEle = nums[i];
                maxIndex = i;
            }
        }

        vector<int> left = vector<int>(nums.begin(), nums.begin() + maxIndex);
        vector<int> right = vector<int>(nums.begin() + maxIndex + 1, nums.end());

        TreeNode *root = new TreeNode(maxEle);
        root->left = constructMaximumBinaryTree(left);
        root->right = constructMaximumBinaryTree(right);

        return root;
    }
};
```
**运行时间：**约48ms，超过64.53%的CPP代码。

## Summary
&emsp;&emsp;这道题给我们提供了一种构建二叉树的思路，由数组构建。其实本质上两者是互通的，我们对二叉树进行遍历得出数组，也可以通过特定的数组去构建二叉树。充分利用二叉树递归定义的性质，使用递归解决问题。这道题的分析到这里，谢谢您的支持！
