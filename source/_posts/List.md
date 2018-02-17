---
title: List
tags:
  - STL
  - List
abbrlink: 39907
date: 2018-02-03 15:37:51
---

## Introduction
In the previous posts, I've talked about some kinds of abstract data structures, which are some different version of list. In this post, I'll introduce the **List**.
<!-- more -->

### Definition
A **List** of elements of type *T* is a finite sequence of elements of *T* together with the following operations:
1. **Construct** the list, leaving it empty.
2. Determine whether the list is **empty** or not.
3. Determine whether the list if **full** or not.
4. Find the **size** of the list.
5. **Clear** the list to make it empty.
6. **Insert** an entry at a specified position of the list.
7. **Remove** an entry from a specified position in the list.
8. **Retrieve** the entry from a specified position in the list.
9. **Replace** the entry at a specified position in the list.
10. **Traverse** the list, performing a given operation on each entry.


### Implementation
First I'd like to give the definition of List.(List.hpp) Here I specify the size of list is 100 in brief.
```C++
template <class List_entry>
class List {
public:
  List();
  ~List();
  List(const List<List_entry> &copy);
  void operator=(const List<List_entry> &copy);
  void clear();
  bool empty() const;
  int size() const;
  //operations
  Error_code insert(int position, const List_entry &x);
  Error_code remove(int position, List_entry &x);
  Error_code retrieve(int position, List_entry &x) const;
  Error_code replace(int position, const List_entry &x);
  void traverse(void (*visit)(List_entry &));
protected:
  int count;
  List_entry* entry;
};
```

#### List()
*precondition*: None.
*postcondition*: The *List* has been created and is initialized to be empty.
```C++
template <class List_entry>
List<List_entry>::List() {
  count = 0;
  entry = new List_entry[100];
  for (int i = 0; i < 100; i++) {
    entry[i] = 0;
  }
}
```
#### ~List()
*precondition*: None.
*postcondition*: Clear the *List* itself.
```C++
template <class List_entry>
List<List_entry>::~List() {
  delete []entry;
}
```

#### List(const List< List_entry > &copy)
*precondition*: None.
*postcondition*: The *List* is initialized as a copy of *List* copy.
```C++
template <class List_entry>
List<List_entry>::List(const List<List_entry> &copy) {
  count = copy.count;
  this->entry = new List_entry[100];
  for (int i = 0; i < 100; i++) {
    this->entry[i] = copy.entry[i];
  }
}
```

#### operator=(const List< List_entry > &copy)
*precondition*: None.
*postcondition*: The *List* is reset as a copy of *List* copy.
```C++
template <class List_entry>
void List<List_entry>::operator=(const List<List_entry> &copy) {
  count = copy.count;
  for (int i = 0; i < count; i++) {
    this->entry[i] = copy.entry[i];
  }
}
```

#### clear()
*precondition*: None.
*postcondition*: All *List* entries have been removed; the *List* is empty.
```C++
template <class List_entry>
void List<List_entry>::clear() { count = 0; }
```

#### empty()
*precondition*: None.
*postcondition*: The function returns **true** or **false** according to whether the *List* is empty or not.
```C++
template <class List_entry>
bool List<List_entry>::empty() const { return count == 0; }
```

#### size()
*precondition*: None.
*postcondition*: The function returns the number of entries in the *List*.
```C++
template <class List_entry>
int List<List_entry>::size() const { return count; }
```

#### insert(int position, **const** List_entry &x)
*precondition*: None.
*postcondition*: If the *List* is not full and 0 ≤ position ≤ n, where n is the number of entries in the *List*, the function succeeds: Any entry formerly at position and all later entries have their position numbers increased by 1, and x is inserted at position in the *List*.  Else: The function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code List<List_entry>::insert(int position, const List_entry &x) {
  if (position < 0 || position > count)
    return Range_over;
  for (int i = count -1; i>= position; i--) {
    entry[i + 1] = entry[i];
  }
  entry[position] = x;
  count++;
  return Success;
}
```

#### remove(int position, List_entry &x)
*precondition*: None.
*postcondition*: If 0 ≤ position < n, where n is the number of entries in the *List*, the function succeeds: The entry at position is removed from the *List*, and all later entries have their position numbers decreased by 1. The parameter x records a copy of the entry formerly at position. Else: The function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code List<List_entry>::remove(int position, List_entry &x) {
  if (empty())
    return UnderFlow;
  if (position < 0 || position >= count)
    return Range_over;
  x = entry[position];
  for (int i = position; i < count; i++)
  {
    entry[i] = entry[i + 1];
  }
  count--;
  return Success;
}
```

#### retrieve(int position, List_entry &x)
*precondition*: None.
*postcondition*: If 0 ≤ position < n, where n is the number of entries in the *List*, the function succeeds: The entry at position is copied to x; all *List* entries remain unchanged. Else: The function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code List<List_entry>::retrieve(int position, List_entry &x) const {
  if (empty())
    return UnderFlow;
  if (position < 0 || position >= count)
    return Range_over;
  x = entry[position];
  return Success;
}
```

#### replace(int position, **const** List_entry &x)
*precondition*: None.
*postcondition*: If 0 ≤ position < n, where n is the number of entries in the *List*, the function succeeds: The entry at position is replaced by x; all other entries remain unchanged. Else: The function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code List<List_entry>::replace(int position, const List_entry &x)
{
  if (empty())
    return UnderFlow;
  if (position < 0 || position >= count)
    return Range_over;
  entry[position] = x;
  return Success;
}
```

#### traverse(void (*visit)(List_entry &))
*precondition*: None.
*postcondition*: The action specified by function \*visit has been performed on every entry of the *List*, beginning at position 0 and doing each in turn.
```C++
template <class List_entry>
void List<List_entry>::traverse(void (*visit)(List_entry &)) {
  for (int i = 0; i < count; i++)
    (*visit)(entry[i]);
}
```
So far, I've show you the List. List is an important data structure, which is the base of Stack and Queue. Consequently, use List expertly will be helpful for further learing. Thank you!
