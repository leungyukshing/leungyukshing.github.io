---
title: LeetCode解题报告（448）-- 1660. Correct a Binary Tree
mathjax: true
date: 2021-12-24 22:16:29
---

## Problem

You have a binary tree with a small defect. There is **exactly one** invalid node where its right child incorrectly points to another node at the **same depth** but to the **invalid node's right**.

Given the root of the binary tree with this defect, `root`, return *the root of the binary tree after **removing** this invalid node **and every node underneath it** (minus the node it incorrectly points to).*

<!-- more -->

**Custom testing:**

The test input is read as 3 lines:

- `TreeNode root`
- `int fromNode` (**not available to** `correctBinaryTree`)
- `int toNode` (**not available to** `correctBinaryTree`)

After the binary tree rooted at `root` is parsed, the `TreeNode` with value of `fromNode` will have its right child pointer pointing to the `TreeNode` with a value of `toNode`. Then, `root` is passed to `correctBinaryTree`.

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2020/10/22/ex1v2.png)

```
Input: root = [1,2,3], fromNode = 2, toNode = 3
Output: [1,null,3]
Explanation: The node with value 2 is invalid, so remove it.
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2020/10/22/ex2v3.png)

```
Input: root = [8,3,1,7,null,9,4,2,null,null,null,5,6], fromNode = 7, toNode = 4
Output: [8,3,1,null,null,9,4,null,null,5,6]
Explanation: The node with value 7 is invalid, so remove it and the node underneath it, node 2.
```

**Constraints:**

- The number of nodes in the tree is in the range `[3, 104]`.
- `-109 <= Node.val <= 109`
- All `Node.val` are **unique**.
- `fromNode != toNode`
- `fromNode` and `toNode` will exist in the tree and will be on the same depth.
- `toNode` is to the **right** of `fromNode`.
- `fromNode.right` is `null` in the initial tree from the test data.

------

## Analysis

&emsp;&emsp;今天连续第三道二叉树的题目了，但这题和前两题就不太一样。这里题目说有一个非法的节点，它的右指针指向了一个和它同深度但在它右手边的节点，题目要求我们把这个非法的节点及其子树都删掉。

&emsp;&emsp;先归纳一下，题目一共有两部分，第一部分是怎么找到这个非法的节点；第二部分是怎么把这个节点及其子树删除。我们先来看第二部分，一般这种不止删除一个节点，而是整一个子树删掉的，都是可以采用递归的方法，也就是说我只需要返回`nullptr`，就相当于把这个节点删掉。

&emsp;&emsp;接着我们再集中精力来看如何找到这个非法的节点，首先它指向的节点，肯定也是某个节点的子节点，所以说这个被指向的节点实际上入度为2，我们可以利用这个特点去找到这个非法的节点。因为被指向的节点在非法节点的右边，我们可以采用后序遍历，那么**当某个节点它的右指针指向一个被遍历过的节点，说明这个节点就是非法的**。

## Solution

&emsp;&emsp;因为题目说树中节点的值都是唯一的，所以我就用`unordered_set<int>`去记录已经访问过的节点。

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
    TreeNode* correctBinaryTree(TreeNode* root) {
        return helper(root);
    }
private:
    unordered_set<int> visited;
    TreeNode* helper(TreeNode* root) {
        if (!root) {
            return nullptr;
        }
        
        if (root->right && visited.count(root->right->val)) {
            return nullptr;
        }
        
        root->right = helper(root->right);
        root->left = helper(root->left);
        
        visited.insert(root->val);
        return root;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是二叉树中比较新鲜的题目，但是也不困难。这里我们主要掌握了两个点，一个是如果删除子树，可以通过在递归中返回`nullptr`快速实现，第二个是可以利用后序遍历去先遍历右子树。这道题目的分享到这里，感谢你的支持！
