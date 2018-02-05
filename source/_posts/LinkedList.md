---
title: LinkedList
tags:
  - STL
  - List
abbrlink: 32198
date: 2018-02-03 22:36:55
---

## Introduction
This post will show you a List implemented by using dynamic storage.
Noted that we use the Node structure the same as which we have used in the previous containers. If you still have question about that, you can search **Node** in this station.
<!-- more -->

### Implementation
Here I give the definition of LinkedList.(LinkedList.hpp)
```C++
template <class List_entry>
class LinkedList {
public:
	LinkedList();
	int size() const;
	//bool full() const;
	bool empty() const;
	void clear();
	void traverse(void (*visit)(List_entry&));
	Error_code retrieve(int position, List_entry &x) const;
	Error_code replace(int position, const List_entry &x);
	Error_code remove(int position, List_entry &x);
	Error_code insert(int position, const List_entry &x);
	~LinkedList();
	LinkedList(const LinkedList<List_entry> &copy);
	void operator=(const LinkedList<List_entry> &copy);
private:
	int count;
	Node<List_entry> *head;

	//auxiliary function used to locate list positions
	Node<List_entry> *set_position(int position) const;
};
```
Please pay attention that I have included an auxiliary function **set_position()** which will prove useful in our implementations of the methods.

#### LinkedList()
*precondition*: None.
*postcondition*: The *List* has been created and is initialized to be empty.
``` C++
template <class List_entry>
LinkedList<List_entry>::LinkedList() {
	count = 0;
	head = NULL;
}
```

#### size()
*precondition*: None.
*postcondition*: The function returns the number of items in the *List*.
``` C++
template <class List_entry>
int LinkedList<List_entry>::size() const { return count; }
```

#### empty()
*precondition*: None.
*postcondition*: The function returns **true** or **false** according to whether the *List* is empty or not.
``` C++
template <class List_entry>
bool LinkedList<List_entry>::empty() const { return count == 0; }
```

#### clear()
*precondition*: None.
*postcondition*: All *List* items have been removed; the *List* is empty.
``` C++
template <class List_entry>
void LinkedList<List_entry>::clear() {
	Node<List_entry>* p1 = head;
	while (p1 != NULL)
	{
		Node<List_entry>* temp = p1;
		p1 = p1->next;
		delete temp;
	}
	head = NULL;
	count = 0;
}
```

#### traverse(void (*visit)(List_entry&))
*precondition*: None.
*postcondition*: The action specified by function *visit has been performed on every item of the List, beginning at position 0 and doing each in turn.
``` C++
template <class List_entry>
void LinkedList<List_entry>::traverse(void (*visit)(List_entry&)) {
	Node<List_entry>* p1 = head;

	for (int i = 0; i < count; i++) {
		(*visit)(p1->entry);
		p1 = p1->next;
	}
}
```

#### retrieve(int position, List_entry &x)
*precondition*: None.
*postcondition*: If 0 ≤ position < n, where n is the number of items in the *List*, the function succeeds: The item at position is copied to x; all *List* items remain unchanged. Else: The function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code LinkedList<List_entry>::retrieve(int position, List_entry &x) const {
  if (empty()) {
    return UnderFlow;
  }
  if (position < 0 || position >= count) {
    return Range_over;
  }
  Node<List_entry>* p = set_position(position);
  x = p->entry;
  return Success;
}
```

#### replace(int position, **const** List_entry &x)
*precondition*: None.
*postcondition*: If 0 ≤ position < n, where n is the number of items in the *List*, the function succeeds: The item at position is replaced by x; all other items remain unchanged. Else: The function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code LinkedList<List_entry>::replace(int position, const List_entry &x) {
  if (empty()) {
    return UnderFlow;
  }
  if (position < 0 || position >= count) {
    return Range_over;
  }
  Node<List_entry>* p = set_position(position);
  p->entry = x;
  return Success;
}
```

#### remove(int position, List_entry &x)
*precondition*: None.
*postcondition*: If 0 ≤ position < n, where n is the number of items in the *List*, the function succeeds: The item at position is removed from the *List*. The parameter x records a copy of the item formerly at position. Else: The function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code LinkedList<List_entry>::remove(int position, List_entry &x) {
  if (empty()) {
    return UnderFlow;
  }
  if (position < 0 || position >= count) {
    return Range_over;
  }
  if (count == 1) {
    delete head;
    head = NULL;
    count = 0;
    return Success;
  }
  Node<List_entry>* pre = set_position(position - 1);
  Node<List_entry>* p = set_position(position);
  pre->next = p->next;
  x = p->entry;
  delete p;
  count--;
  return Success;
}
```

#### insert(int position, const List_entry &x)
*precondition*: None.
*postcondition*: If the *List* is not full and 0 ≤ position ≤ n, where n is the number of items in the *List*, the function succeeds: x is inserted at position of the *List*. Else: the function fails with a diagnostic error code.
```C++
template <class List_entry>
Error_code LinkedList<List_entry>::insert(int position, const List_entry &x) {
  if (position < 0 || position > count) {
    //cout << "hello" << endl;
    return Range_over;
  }
  if (position == 0) {
    Node<List_entry>* p = new Node<List_entry>(x);
    p->next = head;
    head = p;
    count++;
    return Success;
  }
  else {
    Node<List_entry>* p = new Node<List_entry>(x);
    Node<List_entry>* pre = set_position(position - 1);
    p->next = pre->next;
    pre->next = p;
    count++;
    return Success;
  }
}
```

#### ~LinkedList()
*precondition*: None.
*postcondition*: Clear the *List* itself.
``` C++
template <class List_entry>
LinkedList<List_entry>::~LinkedList() {
	count = 0;
	clear();
}
```

#### LinkedList(const LinkedList< List_entry > &copy)
*precondition*: None.
*postcondition*: The *List* is initialized as a copy of *List* copy.
``` C++
template <class List_entry>
LinkedList<List_entry>::LinkedList(const LinkedList<List_entry> &copy) {
	if (copy.head == NULL) {
		this->head = NULL;
		this->count = 0;
	}
	else {
		Node<List_entry>* p2 = copy.head;
		this.head = new Node<List_entry>(p2->entry);
		p2 = p2->next;
		Node<List_entry>* p1 = this.head;
		this.count = copy.count;

		while (p2 != NULL) {
			p1->next = new Node<List_entry>(p2->entry);
			p1 = p1->next;
			p2 = p2->next;
		}
	}
}
```

#### operator=(const LinkedList< List_entry > &copy)
*precondition*: None.
*postcondition*: The *List* is reset as a copy of *List* copy.
``` C++
template <class List_entry>
void LinkedList<List_entry>::operator=(const LinkedList<List_entry> &copy) {
  if (*this == copy) {
    return;
  }
  clear();
  if (copy.head == NULL) {
    this->head = NULL;
    this->count = 0;
  }
  else {
    Node<List_entry>* p2 = copy.head;
    this.head = new Node<List_entry>(p2->entry);
    p2 = p2->next;
    Node<List_entry>* p1 = this.head;
    this.count = copy.count;

    while (p2 != NULL) {
      p1->next = new Node<List_entry>(p2->entry);
      p1 = p1->next;
      p2 = p2->next;
    }
  }
}
```

#### set_position(int position)
*precondition*: position is a valid position in the *List*; 0 ≤ position < count.
*postcondition*: Returns a pointer to the Node in position.
``` C++
template <class List_entry>
Node<List_entry> *LinkedList<List_entry>::set_position(int position) const {
  Node<List_entry>* p1 = head;
  if (position < 0 || position >= count) {
    return NULL;
  }

  while (position--) {
    p1 = p1->next;
  }
  return p1;
}
```

Above is the definition and its implementations of a LinkedList. In this post, I'd like to make a comparation between a *contiguous* list and a *linked* list concerning to their time complexity.
The contiguous list:
  + **insert** and **remove** require time approximately proportional to **n**(**O(n)**).
  + **List**, **clear**, **empty**, **full**, **size**, **replace**, and **retrieve** operate in constant time(**O(1)**).

The linked list:
  + **clear**, **insert**, **remove**, **retrieve**, and **replace** require time approximately proportional to **n**(**O(n)**).
  + **List**, **empty**, **full**, and **size** operate in constant time(**O(1)**).

Noted that I havn't included **traverse** in this discussion, since its time depends on the time used by its parameter **visit**, which we don't know.

This is all the content of this post. Hope it can be useful to your. Thank you very much!
