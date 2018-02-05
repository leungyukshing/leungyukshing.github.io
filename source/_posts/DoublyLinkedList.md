---
title: DoublyLinkedList
tags:
  - STL
  - List
abbrlink: 63548
date: 2018-02-04 21:52:46
---

## Introduction
In the last post, I've introduced the linked list. But there is one problem that we have to traversing the list from its beginning until the expected node was found, which is time-wasting. In this post, I'll give a new way to solve this problem by using a two links node.
<!-- more -->

### Implementation
By defining a **two links** node, it is handy for us to move in either direction through the linked list with equal ease. Here  I'll show you the definition of **Node**.
```C++
template <class Node_entry>
struct Node {
  // data numbers
  Node_entry entry;
  Node<Node_entry> *next;
  Node<Node_entry> *back;
  // constructors
  Node() {
    next = back = NULL;
  }
  Node(Node_entry new_entry, Node<Node_entry> *link_back = NULL,
   Node<Node_entry> *link_next = NULL) {
    entry = new_entry;
    next = link_next;
    back = link_back;
  }
};
```
Now I'll show you the definition of DoublyLinkedList.(DoublyLinkedList.hpp)
```C++
template <class List_entry>
class DoublyLinkedList {
public:
  DoublyLinkedList();
  ~DoublyLinkedList();
  DoublyLinkedList(const DoublyLinkedList<List_entry> &copy);
  void operator=(const DoublyLinkedList<List_entry> &copy);

  int size() const;
  bool full() const;
  bool empty() const;
  void clear();
  void traverse(void (*visit)(List_entry &));
  Error_code retrieve(int position, List_entry &x) const;
  Error_code replace(int position, const List_entry &x);
  Error_code remove(int position, List_entry &x);
  Error_code insert(int position, const List_entry &x);

private:
  int count;
  mutable int current_position;
  mutable Node<List_entry> *current;
  Node<List_entry> *first, *last;
  // The auxiliary function to locate list positions follows
  void set_position(int position) const;
};
```
In this implementtation, it is possible to move in either direction through the *List* while keeping only one pointer into the *List*. Therefore, in the declaration, we keep only a pointer to the current node of the *List*. We do not even need to keep pointers to the head or the tail of the *List*, since they, like any other nodes, can be found by tracing back or forth from any given node.

#### DoublyLinkedList()
*precondition*: None.
*postcondition*: The *List* has been created and is initialized to be empty.
```C++
template <class List_entry>
DoublyLinkedList<List_entry>::DoublyLinkedList() {
  count = 0;
  first = last = NULL;
  current_position = -1;
}
```

#### ~DoublyLinkedList();
*precondition*: None.
*postcondition*: Clear the *List* itself.
```C++
template <class List_entry>
DoublyLinkedList<List_entry>::~DoublyLinkedList() { clear(); }
```

#### DoublyLinkedList(**const** DoublyLinkedList< List_entry > &copy)
*precondition*: None.
*postcondition*: The *List* is initialized as a copy of *List* copy.
```C++
template <class List_entry>
DoublyLinkedList<List_entry>::DoublyLinkedList(const DoublyLinkedList<List_entry> &copy) {
  count = copy.count;
  Node<List_entry> *new_node, *old_node = copy.current;
  if (old_node == NULL)
    current = NULL;
  else {
    new_node = current = new Node<List_entry>(old_node->entry);
    while (old_node->next != NULL) {
      old_node = old_node->next;
      new_node->next = new Node<List_entry>(old_node->entry, new_node);
      new_node = new_node->next;
    }
    old_node = copy.current;
    new_node = current;
    while (old_node->back != NULL) {
      old_node = old_node->back;
      new_node->back = new Node<List_entry>(old_node->entry, NULL, new_node);
      new_node = new_node->back;
    }
  }
}
```

#### operator=(const DoublyLinkedList< List_entry > &copy)
*precondition*: None.
*postcondition*: The *List* is reset as a copy of *List* copy.
```C++
template <class List_entry>
void DoublyLinkedList<List_entry>::operator=(const DoublyLinkedList<List_entry> &copy) {
  DoublyLinkedList new_copy(copy);
  clear();
  count = new_copy.count;
  current_position = new_copy.current_position;
  current = new_copy.current;
  new_copy.count = 0;
  new_copy.current_position = 0;
  new_copy.current = NULL;
}
```

#### size()
*precondition*: None.
*postcondition*: The function returns the number of items in the *List*.
```C++
template <class List_entry>
int DoublyLinkedList<List_entry>::size() const { return count; }
```

#### full()
*precondition*: None.
*postcondition*: The function returns **true** or **false** according to whether the *List* is full or not.
```C++
template <class List_entry>
bool DoublyLinkedList<List_entry>::full() const { return false; }
```

#### empty()
*precondition*: None.
*postcondition*: The function returns **true** or **false** according to whether the *List* is empty or not.
```C++
template <class List_entry>
bool DoublyLinkedList<List_entry>::empty() const { return count == 0; }
```

#### clear()
*precondition*: None.
*postcondition*: All *List* items have been removed; the *List* is empty.
```C++
template <class List_entry>
void DoublyLinkedList<List_entry>::clear() {
  Node<List_entry> *p, *q;
  if (current == NULL)
    return;
  for (p = current->back; p; p = q) {
    q = p->back;
    delete p;
  }
  for (p = current; p; p = q) {
    q = p->next;
    delete p;
  }
  count = 0;
  first = last = current = NULL;
  current_position = -1;
}
```

#### traverse(void (*visit)(List_entry &))
*precondition*: None.
*postcondition*: The action specified by function *visit has been performed on every item of the List, beginning at position 0 and doing each in turn.
```C++
template <class List_entry>
void DoublyLinkedList<List_entry>::traverse(void (*visit)(List_entry &)) {
  Node<List_entry> *to_visit = first;
  for (; to_visit; to_visit = to_visit->next) {
    (*visit)(to_visit->entry);
  }
}
```

#### retrieve(int position, List_entry &x)
*precondition*: None.
*postcondition*: If 0 ≤ position < n, where n is the number of items in the *List*, the function succeeds: The item at position is copied to x; all *List* items remain unchanged. Else: The function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code DoublyLinkedList<List_entry>::retrieve(int position, List_entry &x) const {
  if (empty())
    return UnderFlow;
  if (position < 0 || position >= count)
    return Range_over;

  set_position(position);
  x = current->entry;
  return Success;
}
```

#### replace(int position, **const** List_entry &x)
*precondition*: None.
*postcondition*: If 0 ≤ position < n, where n is the number of items in the *List*, the function succeeds: The item at position is replaced by x; all other items remain unchanged. Else: The function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code DoublyLinkedList<List_entry>::replace(int position, const List_entry &x) {
  if (empty())
    return UnderFlow;
  if (position < 0 || position >= count)
    return Range_over;

  set_position(position);
  current->entry = x;
  return Success;
}
```

#### remove(int position, List_entry &x)
*precondition*: None.
*postcondition*: If 0 ≤ position < n, where n is the number of items in the *List*, the function succeeds: The item at position is removed from the *List*. The parameter x records a copy of the item formerly at position. Else: The function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code DoublyLinkedList<List_entry>::remove(int position, List_entry &x) {
  Node<List_entry> *old_node, *neighbor;
  if (empty())
    return UnderFlow;
  if (position < 0 || position >= count)
    return Range_over;

  set_position(position);
  old_node = current;
  if (neighbor = current->back)
    neighbor->next = current->next;
  if (neighbor = current->next) {
    neighbor->back = current->back;
    current = neighbor;
  }
  else {
    last = current = current->back;
    current_position--;
  }
  x= old_node->entry;
  delete old_node;
  count--;
  if (position == 0)
    first = current;
  return Success;
}
```

#### insert(int position, **const** List_entry &x)
*precondition*: None.
*postcondition*: If the *List* is not full and 0 ≤ position ≤ n, where n is the number of items in the *List*, the function succeeds: x is inserted at position of the *List*. Else: the function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code DoublyLinkedList<List_entry>::insert(int position, const List_entry &x) {
  Node<List_entry> *new_node, *following, *preceding;
  if (position < 0 || position > count)
    return Range_over;
  if (position == 0) {
    if (count == 0)
      following = NULL;
    else {
      set_position(0);
      following = current;
      }
      preceding = NULL;
  }
  else {
    set_position(position - 1);
    preceding = current;
    following = preceding->next;
  }

  new_node = new Node<List_entry>(x, preceding, following);
  if (new_node == NULL)
    return OverFlow;
  if (preceding != NULL)
    preceding->next = new_node;
  if (following != NULL)
    following->back = new_node;
  current = new_node;
  current_position = position;
  if (position == 0)
    first = current;
  if (position == count)
    last = current;
  count++;
  return Success;
}
```

#### set_position(int position)
*precondition*: position is a valid position in the *List*: 0 ≤ position < count.
*postcondition*: The **current** Node pointer references the Node at position.
```C++
template <class List_entry>
void DoublyLinkedList<List_entry>::set_position(int position) const {
  if (current_position == position)
    return;
  else if (current_position < position) {
    for (; current_position != position; current_position++) {
      current = current->next;
    }
  }
  else {
    for (; current_position != position; current_position--) {
      current = current->back;
    }
  }
}
```

Heroto, I'll show all the content of List. At the end of the post, I'd like to summerize some featrues of the List implemented by contiguous storage and linked storage.
**Contiguous storage** is generally preferable:
  + when the entries are individually very small;
  + when the size of the list is known when the program is written;
  + when few insertions or deletions need to be made except at the end of the list;
  + when random access is important and frequent.

**Linked storage** proves superior
  + when the entries are large;
  + when the size fo the list is not known in advance;
  + when flexibility is needed in inserting, deleting, and rearranging the entries.


So it is advisable for you to have a grip on List and choose the most suitable version when you are writing your program. Thank you!
