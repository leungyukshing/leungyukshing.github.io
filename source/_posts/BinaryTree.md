---
title: BinaryTree
tags:
  - BinaryTree
abbrlink: 26202
date: 2018-02-07 22:32:00
---

<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

## Introduction
&emsp;*Linked Lists* have great advantages of flexibility over the contiguous representation of data structure, but they have one weakness: They are sequential lists; that is, they are arranged so that it is necessary to move through them only one position at a time. In this post, we can overcome this problem by using **trees** as data structure, with methods of pointers.
<!-- more -->

### Definition
&emsp;A **binary tree** is either empty, or it consists of a node called the **root** together with two binary trees called the **left subtree** and the **right subtree** of the root.
Noted that **binary trees** are not the same class as the **2-trees**. Each node in a **2-tree** has either 0 or 2 children, never 1, as can happen with a **binary tree**.

For  **n** nodes, here I give a formular to calculate the number of different binary trees:
$$ \cfrac {C_{2n}^{n}}{n+1} $$

### Tree Traversal
#### What is the order?
&emsp;**Traversal**, moving through all the nodes of the binary tree, visiting each node in turn, is one of the most important operations on a binary tree.
For lists or other data structures we've mentioned previously, the **traversal** operation follows the same order. However, for trees, there are several orders in which we could traverse all the nodes.
At a given node, then, there are three tasks we shall wish to do in some order: We shall visit the node **itself**, we shall traverse its **left subtree***; and we shall traverse its **right subtree**. The key distinction in traversal orders is to decide which node we should visit first, among **the node itself**, **its left subtree**, and **its right subtree**.
#### Standard Traversal Orders
For a traversal task, we denote that a node **V**, its left subtree **L**, and its right subtree **R**. So there are **3** orders we may have:

| preorder | inorder | postorder |
|:--------:|:-------:|:---------:|
| **VLR**  | **LVR** |  **LRV**  |

#### Expression Trees
&emsp;An **expression tree** is built up from the simple operands and operators of an(arithmetical or logical) expression by placing the simple operands as the leaves of a binary tree and the operators as the interior nodes.
&emsp;For each **binary operator**, the left subtree contains all the simple operands and operators in the left operand of the given operator, and the right subtree contains everything in the right operand.
&emsp;For a **unary operator**, one of the two subtrees will be empty.
#### Polish Form
&emsp;The names of the traversal methods are related to the **Polish forms** of the expression. Traversal of an expression tree in **preorder** yields the **prefix form**, in which every operator is written before its operands; **inorder** traversal gives the **infix form**(the customary way that we use in daily life); and **postorder** traversal gives the **postfix form**, in which all operators appear after their operands. Although we may consider the **infix form** convenient to use, the computer prefer a **postfix form** expression, **Reverse Polish** expression to calculate. I will show you how to convert a **infix form** to a **postfix form** and calculate it in another post, but not here.

| Expression |  a+b  | log x | n !  |  a-(b×c)  | (a<b) or (c<d) |
|:----------:|:-----:|:-----:|:----:|:---------:|:--------------:|
|  preorder  | + a b | log x | ! n  | - a × b c | or < a b < c d |
|  inorder   | a + b | log x | n !  | a - b × c | a < b or c < d |
| postorder  | a b + | x log | n !  | a b c × - | a b < c d < or |

### Implementation
First we consider the representation of the nodes that make up a tree. We define a structure **Binary_node**, which has both a left and a right subtree.
```C++
template <class Entry>
struct Binary_node {
  // data members
  Entry data;
  Binary_node<Entry> *left;
  Binary_node<Entry> *right;

  //constructors
  Binary_node();
  Binary_node(const Entry &x);
};
```
And its implementations:
```C++
template <class Entry>
Binary_node<Entry>::Binary_node() {
  left = right = NULL;
}

template <class Entry>
Binary_node<Entry>::Binary_node(const Entry &x) {
  data = x;
  left = right = NULL;
}
```

&emsp;Now we can start to construct a tree. For convenience, here we give most methods that a tree need, but not necessary to implement them right now. This provides us a bluprint to construct any binary tree.
```C++
template <class Entry>
class BinaryTree {
public:
  BinaryTree();
  bool empty() const;
  void preorder(void (*visit)(Entry &));
  void inorder(void (*visit)(Entry &));
  void postorder(void (*visit)(Entry &));
  void double_traverse(void (*visit)(Entry &));
  void level_traverse(void (*visit)(Entry &));
  int size() const;
  void clear();
  int height() const;
  int width() const;
  int leaf_count() const;
  void insert(const Entry &);

  BinaryTree(const BinaryTree<Entry> &original);
  BinaryTree & operator=(const BinaryTree<Entry> &original);
  ~BinaryTree();
protected:
  Binary_node<Entry> *root;
  // aux functions
  void recursive_preorder(Binary_node<Entry> *sub_root, void (*visit)(Entry &));
  void recursive_inorder(Binary_node<Entry> *sub_root, void (*visit)(Entry &));
  void recursive_postorder(Binary_node<Entry> *sub_root, void (*visit)(Entry &));
  int recursive_size(Binary_node<Entry> *sub_root) const;
  int recursive_leaf_count(Binary_node<Entry> *sub_root) const;
  int recursive_height(Binary_node<Entry> *sub_root) const;
  void recursive_insert(Binary_node<Entry> *&sub_root, const Entry &x);
  void recursive_clear(Binary_node<Entry> *&sub_root);
  Binary_node<Entry>* recursive_copy(Binary_node<Entry> *sub_root);
  void recursive_double_traverse(Binary_node<Entry> *sub_root, void (*visit)(Entry &));
};
```
&emsp;It seems a bit more complex but just relax. I'll explain it in details. At first, functions like **height**, **size**, and **clear** are basic functions that tell us some features of a tree. They all can be implemented easily by using recursions. So they have their corresponding auxiliary functions in the **protected field**.
&emsp;Now we move to see the traversal functions. As I've introducted that there are 3 standard traversal orders, there are certainly **three traversal functions**. And of course, they have their corresponding auxiliary functions as well.
&emsp;Noted that the **insert** function here is used to test our basic tree class, it will be overrided when BST or other trees are implemented.

#### BinaryTree()
*precondition*: None.
*postcondition*: The *BinaryTree* has been created and is initialized to be empty.
```C++
template <class Entry>
BinaryTree<Entry>::BinaryTree() {
  root = NULL;
}
```

#### BinaryTree(**const** BinaryTree< Entry > &original)
*precondition*: None.
*postcondition*: The *BinaryTree* has been created and is initialized with *BinaryTree* original.
```C++
template <class Entry>
BinaryTree<Entry>::BinaryTree(const BinaryTree<Entry> &original) {
  root = recursive_copy(original.root);
}
```
#### recursive_copy(Binary_node< Entry > *sub_root)
*precondition*: None.
*postcondition*: Return a node that is initialized with sub_root.
```C++
template <class Entry>
Binary_node<Entry>* BinaryTree<Entry>::recursive_copy(Binary_node<Entry> *sub_root) {
  if (sub_root == NULL)
    return NULL;
  Binary_node<Entry> *temp = new Binary_node<Entry>(sub_root->data);
  temp->left = recursive_copy(sub_root->left);
  temp->right = recursive_copy(sub_root->right);
  return temp;
}
```

#### BinaryTree& operator=(**const** BinaryTree< Entry > &original)
*precondition*: None.
*postcondition*: The *BinaryTree* is reset to copy original.
```C++
template <class Entry>
BinaryTree<Entry>& BinaryTree<Entry>::operator=(const BinaryTree<Entry> &original) {
  BinaryTree<Entry> new_copy(original);
  clear();
  root = new_copy.root;
  new_copy.root = NULL;
  return *this;
}
```

#### ~BinaryTree()
*precondition*: None.
*postcondition*: The *BinaryTree* itself is released.
```C++
template <class Entry>
BinaryTree<Entry>::~BinaryTree() {
  clear();
}
```

#### empty()
*precondition*: None.
*postcondition*: The function returns **true** or **false** according to whether the *BinaryTree* is empty or not.
```C++
template <class Entry>
bool BinaryTree<Entry>::empty() const { return root == NULL; }
```

#### size()
*precondition*: None.
*postcondition*: Return the number of entries in the *BinaryTree*.
```C++
// 计算节点个数
template <class Entry>
int BinaryTree<Entry>::size() const {
  return recursive_size(root);
}
```
#### recursive_size(Binary_node< Entry > *sub_root)
*precondition*: sub_root is either NULL or points to a subtree of the *BinaryTree*.
*postcondition*: Return the number of entries in the subtree rooted at sub_root.
```C++
template <class Entry>
int BinaryTree<Entry>::recursive_size(Binary_node<Entry> *sub_root) const {
  // 若本身为空，不计数
  if (sub_root == NULL)
    return 0;
  else
    return 1 + recursive_size(sub_root->left) + recursive_size(sub_root->right);
}
```

#### height()
*precondition*: None.
*postcondition*: Return the height of the *BinaryTree*.
```C++
// 计算树高
template <class Entry>
int BinaryTree<Entry>::height() const {
  return recursive_height(root);
}
```
#### recursive_height(Binary_node< Entry > *sub_root)
*precondition*: sub_root is either NULL or points to a subtree of the *BinaryTree*.
*postcondition*: Return the height of the subtree.
```C++
template <class Entry>
int BinaryTree<Entry>::recursive_height(Binary_node<Entry> *sub_root) const {
  if (sub_root == NULL)
    return 0;
  int left_height = recursive_height(sub_root->left);
  int right_height = recursive_height(sub_root->right);
  if (left_height > right_height) {
    return left_height + 1;
  }
  else
    return right_height + 1;
}
```

#### width()
*precondition*: None.
*postcondition*: Return the width of the *BinaryTree*.
```C++
template <class Entry>
int BinaryTree<Entry>::width() const {
  if (root == NULL)
    return 0;
  LinkedQueue<Binary_node<Entry>*> current_level;
  current_level.append(root);
  int max_level = 0;
  while (current_level.size() != 0) {
    if (current_level.size() > max_level)
      max_level = current_level.size();
    LinkedQueue <Binary_node<Entry> *> next_level;
    do {
      Binary_node<Entry> *sub_root;
      current_level.retrieve(sub_root);
      if (sub_root->left)
        next_level.append(sub_root->left);
      if (sub_root->right)
        next_level.append(sub_root->right);
      current_level.serve();
    } while (!current_level.empty());
    current_level = next_level;
  }
  return max_level;
}
```

#### leaf_count()
*precondition*: None.
*postcondition*: Return the number of leaves in the *BinaryTree*.
```C++
// 计算叶子节点个数
template <class Entry>
int BinaryTree<Entry>::leaf_count() const {
  return recursive_leaf_count(root);
}
```
#### recursive_leaf_count(Binary_node< Entry > *sub_root)
*precondition*: sub_root is either NULL or points to a subtree of the *BinaryTree*.
*postcondition*: Return the number of leaves in the subtree rooted at sub_root.
```C++
template <class Entry>
int BinaryTree<Entry>::recursive_leaf_count(Binary_node<Entry> *sub_root) const {
  // 若本身是空，不计数
  if (sub_root == NULL)
    return 0;
  // 若本身不为空，且无左右子树，即自己为叶子节点，计数
  if (sub_root->left == NULL && sub_root->right == NULL)
    return 1;
  else
    return recursive_leaf_count(sub_root->left) + recursive_leaf_count(sub_root->right);
}
```

#### clear()
*precondition*: None.
*postcondition*: Clean the *BinaryTree* itself.
```C++
template <class Entry>
void BinaryTree<Entry>::clear() {
  recursive_clear(root);
}
```
#### recursive_clear(Binary_node< Entry > *&sub_root)
*precondition*: sub_root is either NULL or points to a subtree of the *BinaryTree*.
*postcondition*: The subtree has been cleaned.
```C++
template <class Entry>
void BinaryTree<Entry>::recursive_clear(Binary_node<Entry> *&sub_root) {
  Binary_node<Entry> *temp = sub_root;
  if (sub_root == NULL)
    return;
  recursive_clear(sub_root->left);
  recursive_clear(sub_root->right);
  sub_root = NULL;
  delete temp;
}
```

#### preorder(void (*visit)(Entry &))
*precondition*: None.
*postcondition*: Traverse the *BinaryTree* in preorder.
```C++
template <class Entry>
void BinaryTree<Entry>::preorder(void (*visit)(Entry &)) {
  recursive_preorder(root, visit);
}
```
#### recursive_preorder(Binary_node< Entry > \*sub_root, void (\*visit)(Entry &))
*precondition*: sub_root is either NULL or points to a subtree of the *BinaryTree*.
*postcondition*: The subtree has been been traversed in preorder sequence.
```C++
template <class Entry>
void BinaryTree<Entry>::recursive_preorder(Binary_node<Entry> *sub_root, void (*visit)(Entry &)) {
  // 当前节点不为空
  if (sub_root != NULL) {
    (*visit)(sub_root->data);
    recursive_preorder(sub_root->left, visit);
    recursive_preorder(sub_root->right, visit);
  }
}
```

#### inorder(void (*visit)(Entry &))
*precondition*: None.
*postcondition*: Traverse the *BinaryTree* in inorder.
```C++
template <class Entry>
void BinaryTree<Entry>::inorder(void (*visit)(Entry &)) {
  recursive_inorder(root, visit);
}
```

#### recursive_inorder(Binary_node< Entry > \*sub_root, void (\*visit)(Entry &))
*precondition*: sub_root is either NULL or points to a subtree of the *BinaryTree*.
*postcondition*: The subtree has been been traversed in inorder sequence.
```C++
template <class Entry>
void BinaryTree<Entry>::recursive_inorder(Binary_node<Entry> *sub_root, void (*visit)(Entry &)) {
  // 当前节点不为空
  if (sub_root != NULL) {
    recursive_inorder(sub_root->left, visit);
    (*visit)(sub_root->data);
    recursive_inorder(sub_root->right, visit);
  }
}
```

#### postorder(void (*visit)(Entry &))
*precondition*: None.
*postcondition*: Traverse the *BinaryTree* in postorder.
```C++
template <class Entry>
void BinaryTree<Entry>::postorder(void (*visit)(Entry &)) {
  recursive_postorder(root, visit);
}
```

#### recursive_postorder(Binary_node< Entry > \*sub_root, void (\*visit)(Entry &))
*precondition*: sub_root is either NULL or points to a subtree of the *BinaryTree*.
*postcondition*: The subtree has been been traversed in postorder sequence.
```C++
template <class Entry>
void BinaryTree<Entry>::recursive_postorder(Binary_node<Entry> *sub_root, void (*visit)(Entry &)) {
  // 当前节点不为空
  if (sub_root != NULL) {
    recursive_postorder(sub_root->left, visit);
    recursive_postorder(sub_root->right, visit);
    (*visit)(sub_root->data);
  }
}
```

#### insert(**const** Entry &)
*precondition*: None.
*postcondition*: If the *root* is empty, the new entry should be inserted into the *root*, otherwise it should be inserted into the shorter of the two subtrees of the *root*(or into the left subtree if both subtrees have the same height).
```C++
// 插入
template <class Entry>
void BinaryTree<Entry>::insert(const Entry &x) {
  recursive_insert(root, x);
}
```
#### recursive_insert(Binary_node< Entry > \*&sub_root, **const** Entry &x)
*precondition*: sub_root is either NULL or points to a subtree of the *BinaryTree*.
*postcondition*: If subroot is n NULL, x is inserted to this subtree; else, move to left or right subtree.
```C++
template <class Entry>
void BinaryTree<Entry>::recursive_insert(Binary_node<Entry> *&sub_root, const Entry &x) {
  // 若当前节点为空，则插入到当前节点
  if (sub_root == NULL){
    sub_root = new Binary_node<Entry>(x);
  }
  // 若左子树较高，则插入到右边
  else if (recursive_height(sub_root->right) < recursive_height(sub_root->left))
    recursive_insert(sub_root->right, x);
  // 若右子树较高或等高，则插入到左边
  else
    recursive_insert(sub_root->left, x);
}
```

#### double_traverse(void (*visit)(Entry &))
*precondition*: None.
*postcondition*: At each node of the *BinaryTree*, **double-order traversal** function first visits the node, then traverses its left subtree(in double order), then visits the node again, then traverses its right subtree(in double order).
```C++
// 双重遍历
template <class Entry>
void BinaryTree<Entry>::double_traverse(void (*visit)(Entry &)) {
  recursive_double_traverse(root, visit);
}
```

#### recursive_double_traverse(Binary_node<Entry> *sub_root, void (*visit)(Entry &))
*precondition*: None.
*postcondition*: The subtree rooted at sub_root is doubly traversed.
```C++
template <class Entry>
void BinaryTree<Entry>::recursive_double_traverse(Binary_node<Entry> *sub_root, void (*visit)(Entry &x)) {
  if (sub_root != NULL) {
    (*visit)(sub_root->data);
    recursive_double_traverse(sub_root->left, visit);
    (*visit)(sub_root->data);
    recursive_double_traverse(sub_root->right, visit);
  }
}
```

#### level_traverse(void (*visit)(Entry &))
*precondition*: None.
*postcondition*: The *BinaryTree* is traversed level by level, starting from the top. The operation *visit is applied to all entries.
```C++
template <class Entry>
void BinaryTree<Entry>::level_traverse(void (*visit)(Entry &)) {
  Binary_node<Entry> *sub_root;
  if (root != NULL) {
    LinkedQueue<Binary_node<Entry> *> waiting_nodes;
    waiting_nodes.append(root); // 将根节点放入队列
    do {
      waiting_nodes.retrieve(sub_root); // 获取头节点进行处理
      (*visit)(sub_root->data);
      if (sub_root->left)
        waiting_nodes.append(sub_root->left);
      if (sub_root->right)
        waiting_nodes.append(sub_root->right);
      waiting_nodes.serve(); // 拿走已经处理的头
    } while (!waiting_nodes.empty());
  }
}
```

That's the general introduction of **BinaryTree**. In next post, I'll introduce another useful tree structure to you. Thank you!
