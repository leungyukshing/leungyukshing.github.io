---
title: Stack
tags:
  - STL
  - Stack
abbrlink: 15783
date: 2018-01-30 20:50:17
---

## Introduction

This post will give a general introduction of Stack, and some of its basic operations.
### Definition
A stack is a version of a list that is particularly useful in applications involving reversing. In a stack data structure, all insertions and deletions of entries are made at one end, called the top fo the stack, which is also the only position that a user can manipulate the entries.
When it comes to the operations, we talk about push and pop. When we add an item to a stack, we usually say that we **push** it onto the stack. And when we remove an item, we usually say that we **pop** it from the stack. So, noted that the last item pushed onto a stack is always the first data to be removed from the stack. This special property is called last in first out, or LIFO.

<!-- more -->

### Implementation
Now we use C++ to implement a stack.
First I will show you the definition of the stack.(Stack.hpp)

``` C++
const int maxstack = 10;

template <class Stack_entry>
class Stack {
public:
  Stack();
  bool empty() const;
  Error_code pop();
  Error_code top(Stack_entry &item) const;
  Error_code push(const Stack_entry &item);
private:
  int count;
  Stack_entry entry[maxstack];
};
```

In this definition, we have a default constructor, two methods to operate entries, and two methods to check whether it is empty(full) or not.

Since I try to implement a stack structure which is similar to the one in STL, template is used in here.If you have any questions about the usage of template, just search it in this station!

Error_code is a enum variables which is used to handle different error, including overflow, underflow and so on. Try to define a Error_code.hpp and it will be convenient for you to use it in the future.

#### Stack()
*precondition*: None.
*postcondition*: The Stack exists and is initialized to be empty.
``` C++
template <class Stack_entry>
Stack<Stack_entry>::Stack() { count = 0; }
```

#### pop()
*precondition*: None.
*postcondition*: If the Stack is not empty, the top of te Stack is  removed, and return Success. If the Stack is empty, an Error_code of UnderFlow is returned and the Stack is left unchanged.
``` C++
template <class Stack_entry>
Error_code Stack<Stack_entry>::pop() {
  if (count == 0)
    return UnderFlow;
  count--;
  return Success;
}
```

#### push(**const** Stack_entry &item)
*precondition*: None.
*postcondition*: If the Stack is not full, item is added to the top of the Stack, and return Success. If the Stack is full, an Error_code of OverFlow is returned and the Stack is left unchanged.
```C++
template <class Stack_entry>
Error_code Stack<Stack_entry>::push(const Stack_entry &item) {
  if (count >= maxstack)
    return OverFlow;
  entry[count++] = item;
  return Success;
}
```

#### top(Stack_entry &item)
*precondition*: None.
*postcondition*: The top of a noneempty Stack is copied to item. A code of fail is returned if the Stack is empty.
```C++
template <class Stack_entry>
Error_code Stack<Stack_entry>::top(Stack_entry &item) const{
  if (count == 0)
    return UnderFlow;
  item = entry[count - 1];
  return Success;
}
```

#### empty()
*precondition*: None.
*postcondition*: **True** is returned if the Stack is empty, otherwise **false** is return.
```C++
template <class Stack_entry>
bool Stack<Stack_entry>::empty() const {
  return count == 0;
}
```

Hereto, I've introduce the Stack structure. Hope this post can be helpful to you. Thank you!
