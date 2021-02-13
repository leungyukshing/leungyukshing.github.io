---
title: LeetCode解题报告（291）-- 138. Copy List with Random Pointer
tags:
  - LeetCode
mathjax: true
date: 2021-02-13 17:18:02

---

## Problem

A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) of the list. The deep copy should consist of exactly `n` **brand new** nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. **None of the pointers in the new list should point to nodes in the original list**.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return *the head of the copied linked list*.

The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:

- `val`: an integer representing `Node.val`
- `random_index`: the index of the node (range from `0` to `n-1`) that the `random` pointer points to, or `null` if it does not point to any node.

Your code will **only** be given the `head` of the original linked list.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2019/12/18/e1.png)

```
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
```

**Example 2:**

![Example2](https://assets.leetcode.com/uploads/2019/12/18/e2.png)

```
Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]
```

**Example 3:**

![Example3](https://assets.leetcode.com/uploads/2019/12/18/e3.png)

```
Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]
```

**Example 4:**

```
Input: head = []
Output: []
Explanation: The given linked list is empty (null pointer), so return null.
```

**Constraints:**

- `0 <= n <= 1000`
- `-10000 <= Node.val <= 10000`
- `Node.random` is `null` or is pointing to some node in the linked list.

------

## Analysis

&emsp;&emsp;题目要求我们**deep copy**一个链表，这个链表比较特殊，除了`next`指针外，还有一个`random`指针，指向链表中的任意一个元素（或者是NULL）。虽然链表的结构是复杂了，但是理清思路后，这个深拷贝和普通链表没什么不同。

&emsp;&emsp;首先我们不考虑复制`random`部分，只copy`next`指向的节点，这个是非常简单的。之后我们再对每一个节点复制他的`random`指针，这是因为第一次遍历完后，能够保证所有节点都被复制了。因为第二遍复制`random`的时候需要在新旧链表之间建立对应关系，所以这里用到了map。在第一遍遍历的时候就建立好新旧链表直接每个节点的关联关系。

------

## Solution

&emsp;&emsp;无

------

## Code

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (!head) {
            return NULL;
        }
        Node *res = new Node(head->val, NULL, NULL);
        Node *node = res, *cur = head->next;
        unordered_map<Node*, Node*> m;
        m[head] = res;
        while (cur) {
            Node *t = new Node(cur->val, NULL, NULL);
            node->next = t;
            m[cur] = t;
            node = node->next;
            cur = cur->next;
        }
        node = res;
        cur = head;
        while (cur) {
            node->random = m[cur->random];
            node = node->next;
            cur = cur->next;
        }
        return res;
    }
};
```

------

## Summary

&emsp;&emsp;这是一道链表深拷贝的进阶题，虽然增加了一个`random`指针使得链表的结构变得负责，但复制的原理还是一样的。这里也借助了map对`random`部分进行拷贝。这道题目的分享到这里，谢谢您的支持！
