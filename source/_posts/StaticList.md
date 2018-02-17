---
title: StaticList
tags:
  - STL
  - List
abbrlink: 1401
date: 2018-02-10 13:20:49
---
## Introduction
In some old computer languages, pointers or other facilities for dynamic storage allocation are not provided. Lists are often implemented by using an array-based storage. This way to store data sometimes better than the dynamic one. This post will show how to implement linked lists using only integer variables and arrays.
<!-- more -->

### Implementation
First we have to define a **Node** structure, which has an **entry** to store the data itself and a **index** to point to the next node. This structure simulate the pointer in some way. Now I give the definition of **Node**.
```C++
typedef int index;
template <class List_entry>
class Node {
public:
  List_entry entry;
  index next;
};
```
Noted that **index** is actually an int type variable, which is the next index of the present entry.
The general definition of a *StaticList* is similiar to the linked one. But a *StaticList* process more auxiliary functions to help handle entries.
Here I'll give the code.(StaticList.hpp)
```C++
template <class List_entry>
class StaticList {
public:
  StaticList();
  int size() const;
  bool full() const;
  bool empty() const;
  void clear();
  void traverse(void (*visit)(List_entry &));
  Error_code retrieve(int position, List_entry &x) const;
  Error_code replace(int position, const List_entry &x);
  Error_code remove(int position, List_entry &x);
  Error_code insert(int position, const List_entry &x);
protected:
  // Data members
  Node<List_entry> workplace[max_list];
  index available, last_used, head;
  int count;

  // Auxiliary member funtions
  index new_node();
  void delete_node(index n);
  int current_position(index n) const;
  index set_position(int position) const;
};
```
The functions **new_node** and **delete_node** play the roles of the C++ operators **new** and **delete**. Thus, **new_node** returns a previously unallocated index from *workplace*.
The **set_position** operation is used to locate the index of *workplace* that stores the element of out list with a given position number.
The **current_position** operation uses an index in *workplace* as its parameter, it calculates the position of any list entry stored there.

#### StaticList()
*precondition*: None.
*postcondition*: The *StaticList* has been created and is initialized to be empty.
```C++
template <class List_entry>
StaticList<List_entry>::StaticList() {
  head = -1;
  count = 0;
  available = -1;
  last_used = -1;
}
```

#### size()
*precondition*: None.
*postcondition*: The function returns the number of entries in the *StaticList*.
```C++
template <class List_entry>
int StaticList<List_entry>::size() const {
  return count;
}
```

#### full()
*precondition*: None.
*postcondition*: The function returns **true** or **false** according to whether the *StaticList* is full or not.
```C++
template <class List_entry>
bool StaticList<List_entry>::full() const {
  return count == max_list;
}
```

#### empty()
*precondition*: None.
*postcondition*: The function returns **true** or **false** according to whether the *StaticList* is empty or not.
```C++
template <class List_entry>
bool StaticList<List_entry>::empty() const {
  return count == 0;
}
```

#### clear()
*precondition*: None.
*postcondition*: All *List* entries have been removed; the *StaticList* is empty.
```C++
template <class List_entry>
void StaticList<List_entry>::clear() {
  index p, q;
  for (p = head; p != -1; p = q) {
    q = workplace[p].next;
    delete_node(p);
  }
  count = 0;
  head = -1;
}
```

#### traverse(void (*visit)(List_entry &))
*precondition*: None.
*postcondition*: The action specified by function *visit has been performed on every entry of the *StaticList*, beginning at position 0 and doing each in turn.
```C++
template <class List_entry>
void StaticList<List_entry>::traverse(void (*visit)(List_entry &)) {
  for (index n = head; n != -1; n = workplace[n].next)
    (*visit)(workplace[n].entry);
}
```

#### retrieve(int position, List_entry &x)
*precondition*: None.
*postcondition*: If the *StaticList* is not full and 0 < position < n; where n is the number of entries in the *StaticList*, the function succeeds: The entry in position is copied to x. Otherwise the function fails with an error code of Range_over.
```C++
template <class List_entry>
Error_code StaticList<List_entry>::retrieve(int position, List_entry &x) const {
  index current;
  if (position < 0 || position >= count)
    return Range_over;
  current = set_position(position);
  x = workplace[current].entry;
  return Success;
}
```

#### replace(int position, **const** List_entry &x)
*precondition*: None.
*postcondition*: If 0 < position < n; where n is the number of entries in the *StaticList*, the function succeeds: The entry in position is replaced by x, all other entries remain unchanged. Otherwise the function fails with an error code of Range_over.
```C++
template <class List_entry>
Error_code StaticList<List_entry>::replace(int position, const List_entry &x) {
  index current;
  if (position < 0 || position >= count)
    return Range_over;
  current = set_position(position);
  workplace[current].entry = x;
  return Success;
}
```

#### remove(int position, List_entry &x)
*precondition*: None.
*postcondition*: If 0 ≤ position < n, where n is the number of entries in the *StaticList*, the function succeeds: The entry at position is removed from the *StaticList*, and all later entries have their position numbers decreased by 1. The parameter x records a copy of the entry formerly at position. Else: The function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code StaticList<List_entry>::remove(int position, List_entry &x) {
  index prior, current;
  if (count == 0)
    return UnderFlow;
  if (position  < 0 || position >= count)
    return Range_over;
  if (position > 0) {
    prior = set_position(position - 1);
    current = workplace[prior].next;
    workplace[prior].next = workplace[current].next;
  }
  else {
    current = head;
    head = workplace[head].next;
  }
  x = workplace[current].entry;
  delete_node(current);
  count--;
  return Success;
}
```

#### insert(int position, **const** List_entry &x)
*precondition*: None.
*postcondition*: If the *StaticList* is not full and 0 ≤ position ≤ n, where n is the number of entries in the *StaticList*, the function succeeds: Any entry formerly at position and all later entries have their position numbers increased by 1, and x is inserted at position in the *StaticList*.  Else: The function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code StaticList<List_entry>::insert(int position, const List_entry &x) {
  index new_index, previous, following;
  if (position < 0 || position > count)
    return Range_over;
  if (position > 0) {
    previous = set_position(position - 1);
    following = workplace[position].next;
  }
  else {  // insert the entry in the head
    following = head;
  }
  if ((new_index = new_node()) == -1) // Fail to allocate new space
    return OverFlow;
  workplace[new_index].entry = x;
  workplace[new_index].next = following;
  if (position == 0)
    head = new_index;
  else
    workplace[previous].next = new_index;
  count++;
  return Success;
}
```

#### new_node()
*precondition*: None.
*postcondition*: The index of the first available Node in workplace is returned; the data members available, last_used, and workplace are updated as necessary. If the workplace is already full, -1 is returned.
```C++
template <class List_entry>
index StaticList<List_entry>::new_node() {
  index new_index;
  if (available != -1) {
    new_index = available;
    available = workplace[available].next;
  }
  else if (last_used < max_list - 1) {
    new_index = ++last_used;
  }
  else {
    return -1;
  }
  workplace[new_index].next = -1;
  return new_index;
}
```

#### delete_node(index n)
*precondition*: The *StaticList* has a Node stored at index old_index.
*postcondition*: The *StaticList* index old_index is pushed onto the linked stack of available space; available, last_used, and workplace are updated as necessary.
```C++
template <class List_entry>
void StaticList<List_entry>::delete_node(index old_index) {
  index previous;
  if (old_index == head)
    head = workplace[old_index].next;
  else {
    previous = set_position(current_position(old_index) - 1);
    workplace[previous].next = workplace[old_index].next;
  }
  workplace[old_index].next = available;
  available = old_index;
}
```

#### current_position(index n)
*precondition*: None.
*postcondition*: Returns the position number of the node stored at index n, or -1 if there no such node.
```C++
template <class List_entry>
int StaticList<List_entry>::current_position(index n) const {
  int i = 0;
  for (index m = head; m != -1; m = workplace[m].next, i++) {
    if (m == n)
      return i;
  }
  return -1;
}
```

#### set_position(int position)
*precondition*: *position* is a valid position in the *StaticList*; 0 ≤ position < count.
*postcondition*: Returns the index of the node at *position* in the *StaticList*.
```C++
template <class List_entry>
index StaticList<List_entry>::set_position(int position) const {
  index n = head;
  for (int i = 0; i < position; i++, n = workplace[n].next);
    return n;
}
```
So far, I've introduce the **StaticList** and its implementation. Thank you!
