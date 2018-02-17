---
title: HashTable
tags:
  - HashTable
abbrlink: 3513
date: 2018-02-14 14:42:09
---
## Introduction
&emsp;The idea of a **HashTable** is to allow many of the different possible keys that might occur to be mapped to the same location in an array under the action of the index function. Then there will be a possibility that two records will want to be in the same place, but if the number of records that actually occur is small relative to the size of the array, then this possibility will cause little loss of time. Even when most entries in the array are occupied, hash methods can be an effective means of information retrieval.
<!-- more -->

### Hash Function
The two principal criteria in selecting a hash function are as follows:
  + It should be easy and quick to compute.
  + It should achieve an even distribution fo the keys that actually occur across the range of indices.

### Modular Arithmetic
Convert the key to an integer, divide by the size of the index range, and take the remainder as the result.

### Collision Resolution
Hash funciton may map several different key  to the same location in an array, which cause a collision. There are two main solutions to this problem: **Open Addressing** and **Chaining**.
#### Open Addressing
##### 1. Linear Probing
&emsp;The simplest method to cope with a collision is to start with the hash adress and do a sequential search throught the array for the desired key or an empty location. Noted that the table should be built **circular** so that when the last location is reached, the search proceeds to the first location of the array.
##### 2. Quadratic Probing
&emsp;Although linear probing is easy to take out, there is a major drawback, that is, as the table becomes about half full, there is a tendency toward **clustering**, which make it less efficient to find an empty position. So we may use an **increment function** to **reharsh** the index. **Quadratic Probing** is an effective version of increment function. It probes the table at locations *h+1*, *h+4*, *h+9*, ... .
Quadratic probing substantially reduces clustering, but it is not obvious that it will probe all locations in the table, and in fact it does not. When the **hsh_size** is a prime number, quadratic probing reaches half the locations in the table.
##### 3. Random Probing
&emsp;Random probing is also a version of **increment funciton**. We use a pseudorandom number generator to obtain the increment. The random number generator used should be one that always generates the **same sequence** provided it starts with the **same seed**. The **seed** could be specified as some function of the key. This method is excellent in avoiding clustering, but is likely to be slower.

### Implementation
First we give the implementation of **Key** and **Record**.
```C++
class Key: public String {
public:
  char key_letter(int position) const;
  void make_blank();
  Key(const char *copy);
  Key();
};

class Record {
public:
  double get_weight();
  void set_weight(double w);
  char key_letter(int position) const;
  Record(); // default constructor
  operator Key() const; // cast to Key
  void initialize(const Key &a_name); // conversion to Record
private:
  Key name;
  double weight;
};
```
&emsp;The method **key_letter(int position)** extract the character in a particular position of a key. And there is a **vonversion** operator that provides the Key of a Record. Here we may not give the details about how to implement these methods.

Then I give the definition of **HashTable**.(HashTable.hpp)
```C++
class HashTable {
public:
  HashTable();
  void clear();
  Error_code insert(const Record &new_entry);
  Error_code retrieve(const Key &target, Record &found) const;
private:
  Record table[hash_size];
};
```

#### HashTable()
*precondition*: None.
*postcondition*: The *HashTable* has been created and initialized to be empty.
```C++
HashTable::HashTable() {}
```

#### clear()
*precondition*: None.
*postcondition*: The *HashTable* has been cleared and is empty.
```C++
void HashTable::clear() {
  Key null;
  null.make_blank();
  for (int i = 0; i < hash_size; i++) {
    table[i].initialize(null);
  }
}
```

#### insert(**const** Record &new_entry)
*precondition*: None.
*postcondition*: If the *HashTable* is full, a code of OverFlow is returned. If the table already contains an item with the key of new_entry a code of the duplicate_error is returned. Otherwise: The Record new_entry is inserted into the *HashTable* and Success is returned.
```C++
Error_code HashTable::insert(const Record &new_entry) {
  Error_code result = Success;
  int probe_count, // Counter to be sure that table is not full
      increment, // Increment used for quadratic probing
      probe; // Position currently probed in the hash table
  Key null; // null key for comparison purpose
  null.make_blank();
  probe = hash(new_entry);
  probe_count = 0;
  increment = 1;
  while (table[probe] != null  // Is the location empty?
          && table[probe] != new_entry // Dubplicate key?
          && probe_count < (hash_size + 1) / 2) { // Has overflow occurred?
    probe_count++;
    probe = (probe + increment) % hash_size;
    increment += 2; // prepare increment for next iteration
  }
  if (table[probe] == null)
    table[probe] = new_entry; // Insert new entry
  else if (table[probe] == new_entry)
    result = duplicate_error;
  else
    result = Overflow;

  return result;
}
```

#### retrieve(**const** Key &target, Record &found)
*precondition*: None.
*postcondition*: If an entry in the *HashTable* has key equal to *target*, then *found* takes on the value of such an entry, and Success is returned. Otherwise, not_present is returned.
```C++
Error_code HashTable::retrieve(const Key &target, Record &found) const {
  int probe_count, // counter to be sure that table is not full
      increment, // increment used for quadratic probing
      probe; // position currently probed in the hash table
  Key null; // null key for comparison purposes
  null.make_blank();
  probe = hash(target);
  probe_count = 0;
  increment = 1;
  while ((Key)table[probe] != null // Is the location empty?
          && (Key)table[probe] != target // Search Successful?
          && probe_count < (hash_size + 1) / 2) { // Full table?
  probe_count++;
  probe = (probe + increment) % hash_size;
  increment += 2;
  }
  if ((Key)table[probe] == target) {
    found = table[probe];
    return Success;
  }
  else
    return not_present;
}
```
&emsp;Up to now, I have said nothing about **deletion**. Because the way we deal with collision is to probe the table until an empty position is found. So you can never tell that an entry is whether the target you are supposed to find or it is just an entry that has a diferent key. Consequently, I'd like to introduce another way to solve this problem--**Chaining**.
We can take the hash table itself as **an array of linked lists**, so that we can randomly access the keys in the table and **insert** or **delete** for any entry that has the same key.

Here I'll just give the definition of the *HashTable*. I believe it's easy for you to implement it by yourself.
```C++
class HashTable {
public:
  // specified methods here.
private:
  vector<Record> table[hash_size];
};
```
You can **insert** an entry by locating its position with its key and **push_back()** to the list.
You can **delete** an entry by locating its position with its key and **remove()** from the list.

### Analysis of Retrieval Methods
&emsp;In terns of the retrieval methods, I'd like to give a general comparison among three methods--**Sequential Search**, **Binary Search**, **Hash-table Retrieval**.
For a contiguous storage that contains **n** entries, there observations for retrieval is listed as the table below.

|      Methods      |Observations|
|:-----------------:|:----------:|
| Sequential Search |   *O(n)*   |
|   Binary Search   | *O(log n)* |
|HashTable Retrieval|   *O(1)*   |

As we can see, Hash-table is significantly efficient in information retrieval. So make good use of it! Thank you!
