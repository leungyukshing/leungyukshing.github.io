---
title: LeetCode解题报告（215）-- 116. Populating Next Right Pointers in Each Node
tags:
  - LeetCode
mathjax: true
abbrlink: 64566
date: 2020-11-14 20:29:20
---

## Problem

You are given a **perfect binary tree** where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

**Follow up:**

```
You may only use constant extra space.

Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.
```

<!-- more -->

**Example 1:**

![Example 1](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

```
Input: root = [1,2,3,4,5,6,7]
Output: [1,#,2,3,#,4,5,6,7,#]
Explanation: Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
```

**Constraints:**

- The number of nodes in the given tree is less than `4096`.
- `-1000 <= node.val <= 1000`

------

## Analysis

&emsp;&emsp;这道二叉树的题目思路比较新颖，要求在一颗二叉树的基础上，把每个节点的`next`指针指向和它同级的节点。因为是同一层之间的操作，没有跨层的操作，所以实际上这道题目是基于**层次遍历**。每一层处理的时候，跳过最后一个节点，因为最后一个节点的`next`指针为NULL。

------

## Solution

&emsp;&emsp;层次遍历的细节不需要多说，利用queue结合循环进行即可。

------

## Code

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if (!root) {
            return NULL;
        }
        queue<Node*> q;
        q.push(root);
        
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                Node* n = q.front();
                q.pop();
                if (i < size - 1) {
                    Node* nextNode = q.front();
                    n->next = nextNode;
                }
                if (n->left) {
                    q.push(n->left);
                }
                if (n->right) {
                    q.push(n->right);
                }
            }
        }
        return root;
    }
};
```

------

## Summary

&emsp;&emsp;这道题目是二叉树层次遍历的一个变种，虽然题目是比较新的，但是了解到背后的原理后，实际上是一个非常简单的层次遍历。这道题目的分享到这里，谢谢！
