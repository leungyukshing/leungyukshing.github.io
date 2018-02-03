---
title: LinkedStack
tags:
  - STL
  - Stack
abbrlink: 59008
date: 2018-02-01 15:35:19
---

## Introduction
I've introduced the Stack structure in previous post. But considering the efficiency when we operating the entries, you may find that using an array(static one) is not good enough. So in this post, I'll show you a whole new way to store elements——Linked Structure.

The **advantage** of linked structure is that it's convenient to insert or delete element in any position. There won't be a problem about moving elements. But its **setback** is also abvious, that is, you can't randomly access an element unless it is head or back(if possible).
<!-- more -->

## Implementation
First I'll give a definition of a struct variable——Node, which is the basic element in storing data.
```C++
template <class Node_entry>
struct Node {
  // data members
  Node_entry entry;
  Node *next;
  // constructors
  Node() {
  next = NULL;
  }
  Node(Node_entry item, Node *add_on = NULL) {
  entry = item;
  next = add_on;
  }
};
```
Then the definition of Stack is similar to the former one.
```C++
template <class Stack_entry>
class LinkedStack {
public:
  LinkedStack();
  bool empty() const;
  Error_code push(const Stack_entry &item);
  Error_code pop();
  Error_code top(Stack_entry &item) const;
  // Safety features for linked structures
  ~LinkedStack();
  LinkedStack(const LinkedStack<Stack_entry> &orgiginal);
  void operator=(const LinkedStack<Stack_entry> &original);
private:
  Node<Stack_entry>* top_node;
};
```
#### LinkedStack()
*precondition*: None.
*postcondition*: The Stack exists and is initialized to be empty.
```C++
template <class Stack_entry>
LinkedStack<Stack_entry>::LinkedStack() {
	top_node = NULL;
}
```

#### empty()
*precondition*: None.
*postcondition*: **True** is returned if the Stack is empty, otherwise **false** is return.
```C++
template <class Stack_entry>
bool LinkedStack<Stack_entry>::empty() const {
	return top_node == NULL;
}
```

#### push(**const** Stack_entry &item)
*precondition*: None.
*postcondition*: Stack_entry item is added to the top of the Stack; returns Success or returns a code of OverFlow if dynamic memory is exhausted.
```C++
template <class Stack_entry>
Error_code LinkedStack<Stack_entry>::push(const Stack_entry &item) {
	if (top_node == NULL) {
		top_node = new Node<Stack_entry>(item);
	}
	else {
		Node<Stack_entry>* p = top_node;
		while (p->next != NULL) {
			p = p->next;
		}
		Node<Stack_entry>* temp = new Node<Stack_entry>(item);
		p->next = temp;
	}
	return Success;
}
```

#### pop()
*precondition*: None.
*postcondition*: The top of the Stack is removed. If the Stack is empty the method return UnderFlow; otherwise it returns Success.
```C++
template <class Stack_entry>
Error_code LinkedStack<Stack_entry>::pop() {
	if (top_node == NULL) {
		return UnderFlow;
	}
	else if (top_node->next == NULL) {
		delete top_node;
		top_node = NULL;
	}
	else {
		Node<Stack_entry>* p1 = top_node;
		Node<Stack_entry>* p2;
		while (p1->next != NULL) {
			p2 = p1;
			p1 = p1->next;
		}
		p2->next = NULL;
		delete p1;
		return Success;
	}
}
```

#### top(Stack_entry &item)
*precondition*: None.
*postcondition*: The top of a noneempty Stack is copied to item. A code of fail is returned if the Stack is empty.
```C++
template <class Stack_entry>
Error_code LinkedStack<Stack_entry>::top(Stack_entry &item) const {
	if (top_node == NULL) {
		item = -1;
		return UnderFlow;
	}
	else if (top_node->next == NULL) {
		item = top_node->entry;
		return Success;
	}
	else {
		Node<Stack_entry>* p1 = top_node;
		Node<Stack_entry>* p2;
		while (p1->next != NULL) {
			p2 = p1;
			p1 = p1->next;
		}
		//p2->next = NULL;
		item = p1->entry;
		//delete p1;
		return Success;
	}
}
```

#### ~LinkedStack()
*precondition*: None.
*postcondition*: Release the Stack itself and nothing is left.
```C++
template <class Stack_entry>
LinkedStack<Stack_entry>::~LinkedStack() {
	// clear teh stack
	while (!empty()) {
		pop();
	}
}
```

#### LinkedStack(**const** LinkedStack< Stack_entry > &original)
*precondition*: None.
*postcondition*: The Stack is initialized as a copy of Stack original.
```C++
template <class Stack_entry>
LinkedStack<Stack_entry>::LinkedStack(const LinkedStack &original) {
	Node<Stack_entry> *new_copy, *original_node = original.top_node;
	if (original_node == NULL)
		top_node = NULL;
	else {
		top_node = new_copy = new Node<Stack_entry>(original_node->entry);
		while (original_node->next != NULL) {
			original_node = original_node->next;
			new_copy->next = new Node<Stack_entry>(original_node->entry);
			new_copy = new_copy->next;
		}
	}
}
```

#### operator=(**const** LinkedStack< Stack_entry > &original)
*precondition*: None.
*postcondition*: The Stack is reset as a copy of Stack original.
```C++
template <class Stack_entry>
void LinkedStack<Stack_entry>::operator=(const LinkedStack &original) {
	Node<Stack_entry> *new_top, *new_copy, *original_node = original.top_node;
	if (original_node == NULL)
		new_top = NULL;
	else {
		new_copy = new_top = new Node<Stack_entry>(original_node->entry);
		while (original_node->next != NULL) {
			original_node = original_node->next;
			new_copy->next = new Node<Stack_entry>(original_node->entry);
			new_copy = new_copy->next;
		}
	}
	while (!empty()) // Clean out old Stack entries
		pop();
	top_node = new_top; // and replace them with new entries
}
```
