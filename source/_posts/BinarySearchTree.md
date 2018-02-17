---
title: BinarySearchTree
tags:
  - BinaryTree
abbrlink: 8203
date: 2018-02-12 12:42:03
---

## Introduction
&emsp;As you know, sequential can keep the keys in order, searching becomes much faster if we use a contiguous list and binary search. **Binary trees** provide an excellent solution to this problem. By making the entries of an ordered list into the nodes of a binary tree, we shall find that we can search for a target key in *O(log n)* steps, just as with binary search, and we shall obtain algorithms for inserting and deleting entries also in time *O(log n)*.
<!-- more -->

### Definition
A **binary search tree**, or sometimes called **binary sorting tree**, is a binary tree that is either empty or in which every node has a key (within its data entry) and satisfies the following conditions:
  + The key of the root (if it exists) is greater than the key in any node in the left subtree of the root.
  + The key of the root (if it exists) is less than the key in any node in the right subtree of the root.
  + The left and right subtrees of the root are again binary search trees.

Noted that **No two entries in a binary search tree may have equal keys.**

### Implementation
&emsp;We have already introduced C++ declarations that allow us to manipulate binary trees. We use this implementation of binary trees as the base for our **binary search tree** class template.
```C++
template <class Entry>
class BinarySearchTree: public BinaryTree<Entry> {
public:
  Error_code insert(const Entry &new_data);
  Error_code remove(const Entry &target);
  Error_code tree_search(const Entry &target) const;
private:
  // 用于遍历并插入的函数
  Error_code search_and_insert(Binary_node<Entry> *&sub_root, const Entry &new_data);
  // 用于遍历寻找指定节点的函数
  Binary_node<Entry>* search_for_node(Binary_node<Entry> *sub_root, const Entry &target) const;
  // 用于遍历找到要删除节点的函数
  Error_code search_and_remove(Binary_node<Entry> *&sub_root, const Entry &target);
  // 用于处理要删除的根节点的函数
  Error_code remove_root(Binary_node<Entry> *&sub_root);
};
```

#### insert(**const** Entry &new_data)
*precondition*: None.
*postcondition*: If a *Record* with a key matching that of new_data already belongs to the *BinarySearchTree* a code of dublicate_error is returned. Otherwise the *Record* new_data is inserted into the tree in such a way that the properties of a binary search tree are preserved, and a code of Success is returned.
```C++
template <class Entry>
Error_code BinarySearchTree<Entry>::insert(const Entry &new_data) {
  return search_and_insert(this->root, new_data);
}
```

#### search_and_insert(Binary_node< Entry > \*&sub_root, **const** Entry &new_data)
```C++
template <class Entry>
Error_code BinarySearchTree<Entry>::search_and_insert(Binary_node<Entry> *&sub_root, const Entry &new_data) {
  // 若当前节点为空，即找到插入位置，插入元素并返回成功
  if (sub_root == NULL) {
    sub_root = new Binary_node<Entry>(new_data);
    return Success;
  }
  else if (new_data < sub_root->data)
    return search_and_insert(sub_root->left, new_data);
  else if (new_data > sub_root->data)
    return search_and_insert(sub_root->right, new_data);
  else // 重复数据
    return dublicate_error;
}
```

#### remove(**const** Entry &target)
*precondition*: None.
*postcondition*: If a Record with a Key matching that of target belongs to the **BinarySearchTree** a code of Success is returned and the corresponding node is removed from the tree. Otherwise, a code of not_present is returned.
```C++
template <class Entry>
Error_code BinarySearchTree<Entry>::remove(const Entry &old_data) {
  return search_and_remove(this->root, old_data);
}
```

#### search_and_remove(Binary_node< Entry > \*&sub_root, **const** Entry &target)
*precondition*: sub_root is either NULL or points to a subtree of the **BinarySearchTree**.
*postcondition*: If the target is not in the subtree, a code of not_present is returned. Otherwise, a code of Success is returned and the subtree node containing target has been removed in such a way that the properties of a **BinarySearchTree** have been preserved.
```C++
template <class Entry>
Error_code BinarySearchTree<Entry>::search_and_remove(Binary_node<Entry> *&sub_root, const Entry &target) {
  if (sub_root == NULL || sub_root->data == target)
    return remove_root(sub_root);
  else if (target < sub_root->data)
    return search_and_remove(sub_root->left, target);
  else
    return search_and_remove(sub_root->right, target);
}
```

#### remove_root(Binary_node< Entry > *&sub_root)
*precondition*: sub_root is either NULL or points to a subtree of the *BinarySearchTree*.
*postcondition*: If sub_root is NULL, a code of not_present is returned. Otherwise, the root of the subtree is removed in such a way that the properties of a *BinarySearchTree* are preserved. The parameter sub_root is reset as the root of the modified subtree, and Success is returned.
```C++
template <class Entry>
Error_code BinarySearchTree<Entry>::remove_root(Binary_node<Entry> *&sub_root) {
  // Pre: sub_root is either NULL or points to a subtree of the BST
  if (sub_root == NULL)
    return not_present;
  // 用to_delete记录要删除的节点，sub_root就可以移动过了
  Binary_node<Entry> *to_delete = sub_root;
  // 只有左子树
  if (sub_root->right == NULL)
    sub_root = sub_root->left;
  // 只有右子树
  else if (sub_root->left == NULL)
    sub_root = sub_root->right;
  // 左右子树均存在, 找左子树的最右节点
  else {
    to_delete = sub_root->left;
    Binary_node<Entry> *parent = sub_root; // parent of to_delete
    while (to_delete->right != NULL) {
      parent = to_delete;
      to_delete = to_delete->right;
    }
    sub_root->data = to_delete->data;
    // 左子树无右子树
    if (parent == sub_root)
      sub_root->left = to_delete->left;
    else
      parent->right = to_delete->left;
  }
  delete to_delete;
  return Success;
}
```

#### tree_search(**const** Entry &target)
*precondition*: None.
*postcondition*: If there is an entry in the tree whose key matches that in target, the parameter target is replaced by the corresponding record from the tree and a code of Success is returned. Otherwise a code of not_present is returned.
```C++
template <class Entry>
Error_code BinarySearchTree<Entry>::tree_search(const Entry &target) const {
  Error_code result = Success;
  Binary_node<Entry> *found = search_for_node(this->root, target);
  if (found == NULL)
    return not_present;
  else
    target = found->data;
  return result;
}
```

#### search_for_node(Binary_node< Entry > \*sub_root, **const** Entry &target)
*precondition*: sub_root is either NULL or points to a subtree of a *BinarySearchTree*.
*postcondition*: If the target is not in the subtree, a result of NULL is returned. Otherwise, a pointer to the subtree node containing the target is returned.
```C++
template <class Entry>
Binary_node<Entry>* BinarySearchTree<Entry>::search_for_node(Binary_node<Entry> *sub_root, const Entry &target) const {
  // 若当前节点为空或者其数据与目标相符，返回本节点
  if (sub_root == NULL || sub_root->data == target)
    return sub_root;
  else if (target < sub_root->data)
    return search_for_node(sub_root->left, target);
  else
    return search_for_node(sub_root->right, target);
}
```

&emsp;The method **insert** usually insert a new node into a **random BinarySearchTree** with **n** nodes in ***O(log n)*** steps. It is possible, but extremely unlikely, that a random tree may degenerate so that insertions require as many as n steps. If the keys are inserted in sorted order into an empty tree, however, this degenerate case will occur.

And we can use **inorder traversal** to put them out in order.

The above is a general introduction of *BinarySearchTree*, it is useful in storing data and sorting data. Thank you!
