---
title: String
tags:
  - STL
abbrlink: 34330
date: 2018-02-05 14:10:08
---

## Introduction
Before I introduce the String, I'll talk about the **C-strings**. Every **C-string** has type **char ***. Hence, a **C-string** references an **address** in memory; the referenced address is the first of a contiguous set of bytes that store the characters making up the string. The storage occupied by the string must terminate with the special character **'\0'**. The compiler cannot enforce any of these conventions, but any deviation from the rules is likely to cause a **run-time error**, which is considered not encapsulated.
Now it is easy for us to use encapsulation to embed **C-strings** into safer class-based implementation in C++. This makes it more convenient, safe, and significantly efficient.
<!-- more -->

### Definition
At first let's see the code straightly(String.hpp).
```C++
class String {
public:
  String();
  ~String();
  // copy constructor
  String (const String &copy);
  // conversion from C-string
  String (const char *copy);
  // conversion from List
  String (list<char> &copy);
  void clear();
  void setLength(int l);
  int getLength() const;
  void operator=(const String &copy);
  // conversion to C-style string
  const char *c_str() const;
protected:
  char *entries;
  int length;
};

// comparison operators
bool operator==(const String &first, const String &second);
bool operator>(const String &first, const String &second);
bool operator<(const String &first, const String &second);
bool operator>=(const String &first, const String &second);
bool operator<=(const String &first, const String &second);
bool operator!=(const String &first, const String &second);

void strcat(String &add_to, const String &add_on);
void strcpy(String &copy, const String &original);
void strncpy(String &copy, const String &original, int n);
int strstr(const String &text, const String &target);
String read_in(istream &input = cin);
String read_in(int &terminator, istream &input = cin);
void write(String &s);
```
In the implementation, we have to care about these problems: aliases, garbage creation, and uninitialized objects. So an overloaded assignment operator, a copy constructor, a destructor and a constructor are necessary.
For later applicaitons, it will be extremly convenient to be able to apply the comparison operators to determine the **lexicographic relationship** between a pair of strings.
Moreover, the conversion between **C-strings** and **Strings** is also important.

#### String()
*precondition*: None.
*postcondition*: The String is initialized to be empty.
```C++
String::String() {
  length = 0;
  entries = NULL;
}
```

#### ~String()
*precondition*: None.
*postcondition*: Release the *String* itself.
```C++
String::~String() {
  clear();
}
```

#### String (**const** String &copy)
*precondition*: None.
*postcondition*: The *String* is initialized by the *String* copy.
```C++
String::String (const String &copy) {
  this->length = copy.length;
  this->entries = new char[this->length + 1];
  strcpy(this->entries, copy.entries);
  this->entries[this->length] = '\0';
}
```

#### String (**const** char *copy)
*precondition*: The pointer *copy* references a *C-string*
*postcondition*: The *String* is initialized by the *C-string* copy.
```C++
String::String (const char *copy) {
  length = strlen(copy);
  entries = new char[length + 1];
  strcpy(entries, copy);
  entries[length] = '\0';
}
```

#### String (list<char> &copy)
*precondition*: None.
*postcondition*: The *String* is initialized by the character *List* copy.
```C++
String::String (list<char> &copy) {
  length = copy.size();
  entries = new char[length + 1];
  list<char>::iterator it = copy.begin();
  for (int i = 0; i < length; i++) {
    entries[i] = (*it);
    it++;
  }
  entries[length] = '\0';
}
```

#### clear()
*precondition*: None.
*postcondition*: Release the *String* itself.
```C++
void String::clear() {
  if (entries == NULL)
    return;

  length = 0;
  delete []entries;
  entries = NULL;
}
```
#### getLength()
*precondition*: None.
*postcondition*: Return the length of the *String*.
```C++
int String::getLength() const { return length; }
```

#### setLength(int l)
*precondition*: None.
*postcondition*: Set the length of the *String* to *l*.
```C++
void String::setLength(int l) { length = l; }
```

#### operator=(const String &copy)
*precondition*: None.
*postcondition*: The *String* is reset as a copy of *String* copy.
```C++
void String::operator=(const String &copy) {
  if (*this == copy)
    return;

  delete []this->entries;
  this->length = copy.length;
  this->entries = new char[this->length + 1];
  strcpy(this->entries, copy.entries);
  this->entries[this->length] = '\0';
}
```

#### c_str()
*precondition*: None.
*postcondition*: A pointer to a legal *C-string* object matching the *String* is returned.
```C++
const char *String::c_str() const {
  return (const char*) entries;
}
```

#### operator==(**const** String &first, **const** String &second)
*precondition*: None.
*postcondition*: Return **true** if the *String* first **agrees with** *String* second. Else: Return **false**.
```C++
bool operator==(const String &first, const String &second) {
  return strcmp(first.c_str(), second.c_str()) == 0;
}
```

#### operator>(**const** String &first, **const** String &second)
*precondition*: None.
*postcondition*: Return **true** if the *String* first is **greater than** *String* second. Else: Return **false**.
```C++
bool operator>(const String &first, const String &second) {
  return strcmp(first.c_str(), second.c_str()) > 0;
}
```

#### operator<(**const** String &first, **const** String &second)
*precondition*: None.
*postcondition*: Return **true** if the *String* first is **less than** *String* second. Else: Return **false**.
```C++
bool operator<(const String &first, const String &second) {
  return strcmp(first.c_str(), second.c_str()) < 0;
}
```

#### operator>=(**const** String &first, **const** String &second)
*precondition*: None.
*postcondition*: Return **true** if the *String* first is **equal or greater than** *String* second. Else: Return **false**.
```C++
bool operator>=(const String &first, const String &second) {
  return strcmp(first.c_str(), second.c_str()) >= 0;
}
```

#### operator<=(**const** String &first, **const** String &second)
*precondition*: None.
*postcondition*: Return **true** if the *String* first is **equal or less than** *String* second. Else: Return **false**.
```C++
bool operator<=(const String &first, const String &second) {
  return strcmp(first.c_str(), second.c_str()) <= 0;
}
```

#### operator!=(**const** String &first, **const** String &second)
*precondition*: None.
*postcondition*: Return **true** if the *String* first **does not agree with** *String* second. Else: Return **false**.
```C++
bool operator!=(const String &first, const String &second) {
  return strcmp(first.c_str(), second.c_str()) != 0;
}
```

#### strcat(String &add_to, **const** String &add_on)
*precondition*: None.
*postcondition*: The function concatenates *String* add_on onto the end of *String* add_to.
```C++
void strcat(String &add_to, const String &add_on) {
  const char *cfirst = add_to.c_str();
  const char *csecond = add_on.c_str();
  char *copy = new char[strlen(cfirst) + strlen(csecond) + 1];
  strcpy(copy, cfirst);
  strcat(copy, csecond);
  add_to = copy;
  delete []copy;
}
```

#### strcpy(String &copy, **const** String &original)
*precondition*: None.
*postcondition*: The function copies *String* original to *String* copy.
```C++
void strcpy(String &copy, const String &original) {
  if (copy == original)
    return;

  const char *coriginal = original.c_str();
  char *temp = new char[strlen(coriginal) + 1];
  strcpy(temp, coriginal);
  copy = temp;
  delete []temp;
}
```

#### strncpy(String &copy, **const** String &original, int n)
*precondition*: None.
*postcondition*: The function copies at most **n** characters from *String* original to *String* copy.
```C++
void strncpy(String &copy, const String &original, int n) {
  const char *coriginal = original.c_str();
  char *temp = new char[n + 1];
  strncpy(temp, coriginal, n);
  temp[n] = '\0';
  copy = temp;
  delete []temp;
}
```

#### strstr(**const** String &text, **const** String &target)
*precondition*: None.
*postcondition*: If *String* target is a substring of *String* text, the function returns the array index of the first occurrence of the string stored in **target** in the string stored in **test**. Else: The function returns a code of -1.
```C++
int strstr(const String &text, const String &target) {
  const char *ctext = text.c_str();
  const char *ctarget = target.c_str();
  char *pos = strstr(ctext, ctarget);
  int index = pos-ctext;

  return index;
}
```

#### read_in(istream &input = cin)
*precondition*: None.
*postcondition*: Return a *String* read (as characters terminated by a newline or an end-of-file character) from an istream parameter.
```C++
String read_in(istream &input) {
  list<char> temp;
  int size = 0;
  char c;
  while ((c = input.peek()) != EOF && (c = input.get()) != '\n') {
    list<char>::iterator it = temp.begin();
    for (int i = 0; i < size; i++) {
      it++;
    }
    temp.insert(it, c);
    size++;

  }
  String answer(temp);
  return answer;
}
```

#### read_in(int &terminator, istream &input = cin)
*precondition*: None.
*postcondition*: Return a *String* read (as characters terminated by a newline or an end-of-file character) from an *istream* parameter. The terminating character is recorded as the output parameter **terminator**.
```C++
String read_in(int &terminator, istream &input) {
  list<char> temp;
  int size = 0;
  while ((terminator = input.peek()) != EOF && (terminator = input.get()) != '\n') {
    list<char>::iterator it = temp.begin();
    for (int i = 0; i < size; i++) {
      it++;
    }
    temp.insert(it, terminator);
    size++;

  }
  String answer(temp);
  return answer;
}
```

#### write(String &s)
*precondition*: None.
*postcondition*: The *String* parameter is written to cout.
```C++
void write(String &s) {
  cout << s.c_str() << endl;
}
```
Heroto, I've shown you the general definition and the implementation of **String**. Hope this post will be beneficial. Thank you!
