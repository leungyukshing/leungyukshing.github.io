---
title: LeetCode解题报告（239）-- 173. Binary Search Tree Iterator
tags:
  - LeetCode
mathjax: true
abbrlink: 1807
date: 2020-12-10 01:15:06
---

## Problem

Implement the `BSTIterator` class that represents an iterator over the **in-order traversal** of a binary search tree (BST):

- `BSTIterator(TreeNode root)` Initializes an object of the `BSTIterator` class. The `root` of the BST is given as part of the constructor. The pointer should be initialized to a non-existent number smaller than any element in the BST.
- `boolean hasNext()` Returns `true` if there exists a number in the traversal to the right of the pointer, otherwise returns `false`.
- `int next()` Moves the pointer to the right, then returns the number at the pointer.

Notice that by initializing the pointer to a non-existent smallest number, the first call to `next()` will return the smallest element in the BST.

You may assume that `next()` calls will always be valid. That is, there will be at least a next number in the in-order traversal when `next()` is called.

<!-- more -->

**Example 1:**

![Example 1](https://assets.leetcode.com/uploads/2018/12/25/bst-tree.png)

```
Input
["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
Output
[null, 3, 7, true, 9, true, 15, true, 20, false]

Explanation
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next();    // return 3
bSTIterator.next();    // return 7
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 9
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 15
bSTIterator.hasNext(); // return True
bSTIterator.next();    // return 20
bSTIterator.hasNext(); // return False
```

**Constraints:**

- The number of nodes in the tree is in the range `[1, 105]`.
- `0 <= Node.val <= 106`
- At most `105` calls will be made to `hasNext`, and `next`.

 

**Follow up:**

- Could you implement `next()` and `hasNext()` to run in average `O(1)` time and use `O(h)` memory, where `h` is the height of the tree?

------

## Analysis

&emsp;&emsp;这道题目要求我们实现一个二叉树迭代器，顺序是**中序遍历**。BST的中序遍历并不复杂，在前面用递归实现了很多次了，但是在这里，遍历的每一步都是**可暂停**的，所以并不能够用递归一次过做完。于是思路就很直接地指向了使用stack进行中序遍历。

&emsp;&emsp;因为这里的迭代器是一步一步进行的，所以在遍历的时候处理也要分开。首先把左节点都加到stack中，然后在遍历的时候，每处理一个节点，处理完后就需要把这个节点的右节点放入stack中。

------

## Solution

&emsp;&emsp;原理和使用stack+循环中序遍历BST是一样的。

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
class BSTIterator {
public:
    BSTIterator(TreeNode* root) {
        while (root) {
            s.push(root);
            root = root->left;
        }
    }
    
    int next() {
        TreeNode* node = s.top();
        int result = node->val;
        s.pop();
        if (node->right) {
            node = node->right;
            while (node) {
                s.push(node);
                node = node->left;
            }
        }
        return result;
    }
    
    bool hasNext() {
        return !s.empty();
    }
private:
    stack<TreeNode*> s;
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```

------

## Summary

&emsp;&emsp;这道题目本质考察的是BST的中序遍历，以往为了coding方便我都会使用递归，但是这里就要求用stack了。因此在练习的时候使用多种方法是有好处的，可以更好地理解BST的遍历过程。这道题目的分享到这里，谢谢您的支持！
