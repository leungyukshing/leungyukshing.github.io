---
title: LeetCode解题报告（458）-- 655. Print Binary Tree
mathjax: true
date: 2021-12-26 00:17:21
---

## Problem

Given the `root` of a binary tree, construct a **0-indexed** `m x n` string matrix `res` that represents a **formatted layout** of the tree. The formatted layout matrix should be constructed using the following rules:

- The **height** of the tree is `height` and the number of rows `m` should be equal to `height + 1`.
- The number of columns `n` should be equal to `2height+1 - 1`.
- Place the **root node** in the **middle** of the **top row** (more formally, at location `res[0][(n-1)/2]`).
- For each node that has been placed in the matrix at position `res[r][c]`, place its **left child** at `res[r+1][c-2height-r-1]` and its **right child** at `res[r+1][c+2height-r-1]`.
- Continue this process until all the nodes in the tree have been placed.
- Any empty cells should contain the empty string `""`.

Return *the constructed matrix* `res`.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2021/05/03/print1-tree.jpg)

```
Input: root = [1,2]
Output: 
[["","1",""],
 ["2","",""]]
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2021/05/03/print2-tree.jpg)

```
Input: root = [1,2,3,null,4]
Output: 
[["","","","1","","",""],
 ["","2","","","","3",""],
 ["","","4","","","",""]]
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 210]`.
- `-99 <= Node.val <= 99`
- The depth of the tree will be in the range `[1, 10]`.

---

## Analysis

&emsp;&emsp;题目要求我们以数组的形式打印一颗二叉树。数组的长、宽都已经给出，都是和深度有关。然后每个节点和父节点的相对位置也给出。首先我们计算出整棵树的深度，然后我们就开始做一遍前序遍历，传入节点在矩阵中的横坐标和纵坐标。

&emsp;&emsp;这里提一点，虽然题目也给出了计算公式，但是我建议按照自己的理解，在纸上画出高度为3的例子，就一目了然了。

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
    vector<vector<string>> printTree(TreeNode* root) {
        maxDepth = depth(root);
        
        vector<vector<string>> result(maxDepth, vector<string>(pow(2, maxDepth) - 1));
        
        helper(root, 0, (pow(2, maxDepth) - 1) / 2, result);
        return result;
    }
private:
    int maxDepth;
    int depth(TreeNode* root) {
        if (!root) {
            return 0;
        }
        
        return max(depth(root->left), depth(root->right)) + 1;
    }
    
    void helper(TreeNode* root, int d, int pos, vector<vector<string>>& result) {
        //cout << d << " " << pos << endl;
        result[d][pos] = to_string(root->val);
        if (root->left) {
            helper(root->left, d + 1, pos - pow(2, maxDepth - d - 2), result);
        }
        if (root->right) {
            helper(root->right, d + 1, pos + pow(2, maxDepth - d - 2), result);
        }
    }
};
```

------

## Summary

&emsp;&emsp;问题不难，在位置细节上需要手动运算下，主要是总结下题目，没有特别的算法难点。这道题目的分享到这里，感谢你的支持！
