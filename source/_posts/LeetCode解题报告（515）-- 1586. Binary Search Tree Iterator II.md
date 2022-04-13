---
title: LeetCode解题报告（515）-- 1586. Binary Search Tree Iterator II
mathjax: true
date: 2022-01-13 23:15:34
---

## Problem

Implement the `BSTIterator` class that represents an iterator over the **[in-order traversal](https://en.wikipedia.org/wiki/Tree_traversal#In-order_(LNR))** of a binary search tree (BST):

- `BSTIterator(TreeNode root)` Initializes an object of the `BSTIterator` class. The `root` of the BST is given as part of the constructor. The pointer should be initialized to a non-existent number smaller than any element in the BST.
- `boolean hasNext()` Returns `true` if there exists a number in the traversal to the right of the pointer, otherwise returns `false`.
- `int next()` Moves the pointer to the right, then returns the number at the pointer.
- `boolean hasPrev()` Returns `true` if there exists a number in the traversal to the left of the pointer, otherwise returns `false`.
- `int prev()` Moves the pointer to the left, then returns the number at the pointer.

Notice that by initializing the pointer to a non-existent smallest number, the first call to `next()` will return the smallest element in the BST.

You may assume that `next()` and `prev()` calls will always be valid. That is, there will be at least a next/previous number in the in-order traversal when `next()`/`prev()` is called.

<!-- more -->

**Example 1:**

![Example1](https://assets.leetcode.com/uploads/2018/12/25/bst-tree.png)

```
Input
["BSTIterator", "next", "next", "prev", "next", "hasNext", "next", "next", "next", "hasNext", "hasPrev", "prev", "prev"]
[[[7, 3, 15, null, null, 9, 20]], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null], [null]]
Output
[null, 3, 7, 3, 7, true, 9, 15, 20, false, true, 15, 9]

Explanation
// The underlined element is where the pointer currently is.
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]); // state is   [3, 7, 9, 15, 20]
bSTIterator.next(); // state becomes [3, 7, 9, 15, 20], return 3
bSTIterator.next(); // state becomes [3, 7, 9, 15, 20], return 7
bSTIterator.prev(); // state becomes [3, 7, 9, 15, 20], return 3
bSTIterator.next(); // state becomes [3, 7, 9, 15, 20], return 7
bSTIterator.hasNext(); // return true
bSTIterator.next(); // state becomes [3, 7, 9, 15, 20], return 9
bSTIterator.next(); // state becomes [3, 7, 9, 15, 20], return 15
bSTIterator.next(); // state becomes [3, 7, 9, 15, 20], return 20
bSTIterator.hasNext(); // return false
bSTIterator.hasPrev(); // return true
bSTIterator.prev(); // state becomes [3, 7, 9, 15, 20], return 15
bSTIterator.prev(); // state becomes [3, 7, 9, 15, 20], return 9
```



**Constraints:**

- The number of nodes in the tree is in the range `[1, 105]`.
- `0 <= Node.val <= 106`
- At most `105` calls will be made to `hasNext`, `next`, `hasPrev`, and `prev`.

 

**Follow up:** Could you solve the problem without precalculating the values of the tree?

---

## Analysis

&emsp;&emsp;和上一题一样，也是实现一个中序遍历的BST迭代器，但是支持的功能多了两个`hasPrev()`和`prev()`。这个倒是比较新鲜，之前碰到过iterator的题目一般都是只向后遍历，很少有同时支持向前向后遍历的，这里第一次碰到就好好总结。

&emsp;&emsp;首先向后遍历的基本功能还是不变的，还是借助stack来实现向后的功能。我们重点来看向前的功能。我用现实中的一个例子来讲解，在看视频的是，我们支持快进和后退，那么这个过程实际的操作是如何的呢？快进好理解，我们不断发送请求，请求到数据帧之后就一直往后走，后退是怎么实现呢？很简单的一个方法就是把播放过的数据帧放到一个buffer中存储，假设我现在播放到`t`时刻，如果我向前倒退20帧，那么我只需要在buffer中把下标向前移动20帧即可。

&emsp;&emsp;所以一个basic的idea就是我们用一个buffer记录遍历过的内容，在这个buffer中维护一个下标，然后对向前、向后两个操作分开处理：

+ 向后：首先检查下标在buffer中是否能后移，如果不能，说明要在当前的stack中拿新的数据；否则，可能是之前回退过，可以使用buffer中的数据；
+ 向前：检查下标在buffer中能否前移，如果不能说明没有更旧的数据，直接返回-1；否则，可以使用buffer中的数据。

## Solution

&emsp;&emsp;我们用一个`vector`来存储已经遍历过的数据，用`idx`表明在buffer中当前遍历的位置，初始化为-1。当处理`next()`时，判断`idx + 1`是否超过buffer的size，如果超过就从stack拿新的数据，并放入到buffer中；否则，获取`buffer[idx + 1]`的数据。无论哪种情况`idx`都要自增。同理，在处理`prev()`时，判断`idx - 1`是否小于0，如果小于说明没有更旧的数据，返回-1（这里题目保证了`prev()`调用的时候肯定是valid，所以可以不写错误处理）；否则，获取`buffer[idx - 1]`，然后`idx`自减。

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
        idx = -1;
        while (root) {
            st.push(root);
            root = root->left;
        }
    }
    
    bool hasNext() {
        if ((idx + 1) >= 0 && (idx + 1) < arr.size()) {
            return true;
        }
        return !st.empty();
    }
    
    int next() {
        if ((idx + 1) >= 0 && (idx + 1) < arr.size()) {
            return arr[++idx]->val;
        }
        TreeNode* node = st.top();
        st.pop();
        int result = node->val;
        arr.push_back(node);
        node = node->right;
        while (node) {
            st.push(node);
            node = node->left;
        }
        ++idx;
        return result;
    }
    
    bool hasPrev() {
        return (idx - 1) >= 0 && (idx - 1) < arr.size();
    }
    
    int prev() {
        return arr[--idx]->val;
    }
private:
    stack<TreeNode*> st;
    vector<TreeNode*> arr;
    int idx;
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * bool param_1 = obj->hasNext();
 * int param_2 = obj->next();
 * bool param_3 = obj->hasPrev();
 * int param_4 = obj->prev();
 */
```

------

## Summary

&emsp;&emsp;这道题目是比较有意思的iterator设计题，它引入了`prev()`的概念，我们基本的处理思路就是用一个额外的数组去存放历史数据，在遍历时优先从buffer中拿数据，buffer中没有了再用stack的。实际上iterator还有很多的变种，比如要支持`top()`这种函数，之后碰到了我再详细分析。这道题目的分享到这里，感谢你的支持！

