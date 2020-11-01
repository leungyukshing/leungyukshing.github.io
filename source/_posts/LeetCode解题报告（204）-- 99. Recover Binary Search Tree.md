---
title: LeetCode解题报告（204）-- 99. Recover Binary Search Tree
tags:
  - LeetCode
mathjax: true
date: 2020-11-01 11:43:28

---

## Problem

You are given the `root` of a binary search tree (BST), where exactly two nodes of the tree were swapped by mistake. *Recover the tree without changing its structure*.

**Follow up:** A solution using `O(n)` space is pretty straight forward. Could you devise a constant space solution?

<!-- more -->

**Example 1:**

![Example 1](https://assets.leetcode.com/uploads/2020/10/28/recover1.jpg)

```
Input: root = [1,3,null,null,2]
Output: [3,1,null,null,2]
Explanation: 3 cannot be a left child of 1 because 3 > 1. Swapping 1 and 3 makes the BST valid.
```

**Example 2:**

![Example 2](https://assets.leetcode.com/uploads/2020/10/28/recover2.jpg)

```
Input: root = [3,1,4,null,null,2]
Output: [2,1,4,null,null,3]
Explanation: 2 cannot be in the right subtree of 3 because 2 < 3. Swapping 2 and 3 makes the BST valid.
```

**Constraints:**

- The number of nodes in the tree is in the range `[2, 1000]`.
- `-2^31 <= Node.val <= 2^31 - 1`

------

## Analysis

&emsp;&emsp;这道题目是二叉树比较新颖的题目，题目给定的BST中，有两个节点的位置错乱了，不满足BST的要求，要求我们恢复这个BST的正确顺序。

&emsp;&emsp;BST的正确顺序实际上就是一个有序性，比较简单的做法是直接对BST进行中序遍历，得到数组之后，肯定有两个位置的顺序是乱的。然后对这个数组排序，再把结果放回到BST中。这个做法不单止是对两个位置错乱的情况有效，对任意数量位置错乱的BST都是有效的。

&emsp;&emsp;但是题目有一个Follow up是要求用$O(1)$的空间复杂度完成，上面说的方法用了额外的空间，所以还有优化的空间。最优的解法应该是用Morris遍历，我还在研究中，这个之后再补充道这里和大家分享。

------

## Solution

&emsp;&emsp;普通的做法要用额外的空间存放中序遍历的结果，因为需要对值进行排序，所以用两个数组分开存放，一个是存Node指针，一个是存值。只需要对值进行排序，按顺序放回到Node指针中即可。

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
    void recoverTree(TreeNode* root) {
        vector<TreeNode*> nodes;
        vector<int> vals;
        inorder(root, nodes, vals);
        sort(vals.begin(), vals.end());
        
        int size = nodes.size();
        for (int i = 0; i < size; i++) {
            nodes[i]->val = vals[i];
        }
    }
private:
    void inorder(TreeNode* root, vector<TreeNode*>& nodes, vector<int>& vals) {
        if (!root) {
            return;
        }
        inorder(root->left, nodes, vals);
        nodes.push_back(root);
        vals.push_back(root->val);
        inorder(root->right, nodes, vals);
    }
};
```

------

## Summary

&emsp;&emsp;当前只分享了这道题目最简单的做法，最优的做法还在学习中。从简单的这个做法中也能得到一些启示，有些时候为了做题的方便，当没有过多限制的时候，可以用额外的空间去简化自己的算法。这道题目的分享到这里，谢谢！
