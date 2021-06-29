---
title: LeetCode解题报告（183）-- 449. Serialize and Deserialize BST
tags:
  - LeetCode
mathjax: true
abbrlink: 56498
date: 2020-10-09 22:45:12
---

## Problem

Serialization is converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a **binary search tree**. There is no restriction on how your serialization/deserialization algorithm should work. You need to ensure that a binary search tree can be serialized to a string, and this string can be deserialized to the original tree structure.

**The encoded string should be as compact as possible.**

<!-- more -->

**Example 1:**

```
Input: root = [2,1,3]
Output: [2,1,3]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `0 <= Node.val <= 104`
- The input tree is **guaranteed** to be a binary search tree.

------

## Analysis

&emsp;&emsp;这道题目是思路比较新的一道题目，要求我们实现encoder和decoder，实现BST和字符串之间的相互转换。这类题目的答案比较开放，只要encode和decode的时候按照我们自己设定的规则，那么就是没有问题的。

&emsp;&emsp;前面接触过二叉树，知道我们并不能只通过前序遍历、中序遍历或者后续遍历去唯一地确定一颗二叉树，至少需要两种顺序才能确定。但是这里我们只有一个字符串，有一种思路就是把前序遍历和中序遍历都存在一个字符串里面，中间隔开就是。这个思路我没有实现，但是应该也是可行的。题目有要求说encoded的字符串尽可能短，所以我就想能不能有更加高效的方法。

&emsp;&emsp;只有前序遍历的结果不能恢复一颗二叉树，是因为有一些节点为空，我们不知道根在哪，但是如果是一颗满二叉树呢？如果每个节点都填上内容，那么我们就可以逐个逐个恢复了。问题一下子就变得简单了，对于空的节点，我们只需要用一个符号表示空就可以了。恢复的时候，遇到这个符号就意味着该节点为空。

------

## Solution

&emsp;&emsp;因为节点的val是数字，所以节点值之间要用空格隔开，C++处理带有空格的字符串没有python等语言这么灵活，之前也提到过，可以善用stringstream。无论是encode还是decode其实就是用递归写一个前序遍历，这部分难度不大。

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
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        ostringstream os;
        doSerialization(root, os);
        return os.str();
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        istringstream is(data);
        return doDeserialization(is);
    }
private:
    void doSerialization(TreeNode* root, ostringstream& os) {
        if (!root) {
            // nil node
            os << "# ";
        } else {
            os << root->val << " ";
            doSerialization(root->left, os);
            doSerialization(root->right, os);
        }
    }
    
    TreeNode* doDeserialization(istringstream& is) {
        string val = "";
        is >> val;
        if (val == "#") {
            return NULL;
        }
        TreeNode* node = new TreeNode(stoi(val));
        node->left = doDeserialization(is);
        node->right = doDeserialization(is);
        return node;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec* ser = new Codec();
// Codec* deser = new Codec();
// string tree = ser->serialize(root);
// TreeNode* ans = deser->deserialize(tree);
// return ans;
```

------

## Summary

&emsp;&emsp;这道题目是二叉树题目中出题思路比较新颖的一道题目，利用满二叉树的方法去decode、encode能提高字符串的压缩率。coding过程中也用到了stringstream，总的来说这道题目还是很有价值的。这道题目的分享到这里，谢谢！
