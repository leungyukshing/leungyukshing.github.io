---
title: LeetCode解题报告（258）-- 1379. Find a Corresponding Node of a Binary Tree in a Clone of That Tree
tags:
  - LeetCode
mathjax: true
date: 2021-01-05 00:51:04

---

## Problem

Given two binary trees `original` and `cloned` and given a reference to a node `target` in the original tree.

The `cloned` tree is a **copy of** the `original` tree.

Return *a reference to the same node* in the `cloned` tree.

**Note** that you are **not allowed** to change any of the two trees or the `target` node and the answer **must be** a reference to a node in the `cloned` tree.

**Follow up:** Solve the problem if repeated values on the tree are allowed.

<!-- more -->

**Example 1:**

<img src="https://assets.leetcode.com/uploads/2020/02/21/e1.png" alt="Example 1" style="zoom:75%;" />

```
Input: tree = [7,4,3,null,null,6,19], target = 3
Output: 3
Explanation: In all examples the original and cloned trees are shown. The target node is a green node from the original tree. The answer is the yellow node from the cloned tree.
```

**Example 2:**

<img src="https://assets.leetcode.com/uploads/2020/02/21/e2.png" alt="Example 2" style="zoom:75%;" />

```
Input: tree = [7], target =  7
Output: 7
```

**Example 3:**

<img src="https://assets.leetcode.com/uploads/2020/02/21/e3.png" alt="Example 3" style="zoom:75%;" />

```
Input: tree = [8,null,6,null,5,null,4,null,3,null,2,null,1], target = 4
Output: 4
```

**Example 4:**

<img src="https://assets.leetcode.com/uploads/2020/02/21/e4.png" alt="Example 4" style="zoom:75%;" />

```
Input: tree = [1,2,3,4,5,6,7,8,9,10], target = 5
Output: 5
```

**Example 5:**

<img src="https://assets.leetcode.com/uploads/2020/02/21/e5.png" alt="Example 5" style="zoom:75%;" />

```
Input: tree = [1,2,null,3], target = 2
Output: 2
```

**Constraints:**

- The number of nodes in the `tree` is in the range `[1, 10^4]`.
- The values of the nodes of the `tree` are unique.
- `target` node is a node from the `original` tree and is not `null`.

------

## Analysis

&emsp;&emsp;这是一道考察点比较新的二叉树题目，但实际上是非常简单的。题目给出两颗完全一样的二叉树，还有一个指向`original`某个节点的指针，要求在`cloned`上面找出指向相同节点的指针。

&emsp;&emsp; 如果只是AC这道题目是非常简单的，因为题目给出了限制二叉树中的值都是唯一的，所以我们可以通过值来判断即可。遍历二叉树，找到和`target`值一样的节点就是要找的节点了。

&emsp;&emsp;但是题目也给了Follow up，如果树里面存在相同值的节点，这道题目又该怎么解呢？可以利用两颗二叉树是一样的这个特点，同时对两颗二叉树进行遍历，通过比对`target`和`original`中节点的**地址**去判断是否找到该节点，找到后，返回当时指向`cloned`的那个节点即可。

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
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Solution {
public:
    TreeNode* getTargetCopy(TreeNode* original, TreeNode* cloned, TreeNode* target) {
        if (!original) {
            return NULL;
        }
        if (original == target) {
            return cloned;
        }
        TreeNode* node = getTargetCopy(original->left, cloned->left, target);
        if (!node) {
            node = getTargetCopy(original->right, cloned->right, target);
        }
        return node;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是比较简单的二叉树题目，在值都是唯一的情况下，题目是比较好解决的；当值不能成为判断的标准，就需要用到地址了。这道题这道题目的分享到这里，谢谢您的支持！
