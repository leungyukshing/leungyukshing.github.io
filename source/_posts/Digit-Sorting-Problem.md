---
title: 数字排序
tags:
  - CCF
abbrlink: 58423
date: 2018-03-09 13:38:09
---
## 数字排序问题
### 问题描述
&emsp;给定n个整数，请统计出每个整数出现的次数，按出现次数从多到少的顺序输出。

### 输入格式
&emsp;输入的第一行包含一个整数n，表示给定数字的个数。
&emsp;第二行包含n个整数，相邻的整数之间用一个空格分隔，表示所给定的整数。

### 输出格式
&emsp;输出多行，每行包含两个整数，分别表示一个给定的整数和它出现的次数。按出现次数递减的顺序输出。如果两个整数出现的次数一样多，则先输出值较小的，然后输出值较大的。
<!-- more -->

### 样例输入
> 12
> 5 2 3 3 1 3 4 2 5 2 3 5

### 样例输出
> 3 4
> 2 3
> 5 3
> 1 1
> 4 1

### 评测用例规模与约定
&emsp;1 ≤ n ≤ 1000，给出的数都是不超过1000的非负整数。

## 解决方法
### 分析
&emsp;这是一个很经典的统计数字出现次数的问题，首先我们想到的传统做法是使用**map**。我们可以把**key**设为是数字，对应的**value**设置为对应的出现次数，这样做是很方便的。但是在本题中，输出格式的顺序不仅与**key**有关，还与**value**有关，因此上述方法就很不方便了。
&emsp;我选择了一种自定义的方法，自己定义一个新的类**Pair**，并根据题目意思重新定义它的排序规则，从而让我能够直接使用**\< algorithm \>**算法库中的**sort**函数进行排序。
&emsp;注意排序规则：
  + 出现次数多的先输出
  + 若出现次数相同，值较小的先输出

### 代码实现
```C++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

struct Pair {
  int key;
  int value;

  Pair(int _key, int _value) {
    key = _key;
    value = _value;
  }

  bool operator<(const Pair& other) const {
    if (value < other.value)
      return true;
    else if (value == other.value && key > other.key)
      return true;

    return false;
  }
};

int main() {
  int n;
  cin >> n;
  int size = n;
  std::vector<Pair> v;

  while (n--) {
    int key;
    cin >> key;

    bool isFound = false;
    for (int i = 0; i < v.size(); i++) {
      if (v[i].key == key) {
        v[i].value++;
        isFound = true;
        break;
      }
    }
    if (!isFound) {
      v.push_back(Pair(key, 1));
    }
  }

  sort(v.begin(), v.end());
  for (int i = v.size() - 1; i >= 0; i--) {
    cout << v[i].key << " " << v[i].value << endl;
  }
  return 0;
}
```
&emsp;这题也给我展示了一个很常见的编程思路，在我们需要对**object**分先后的时候，可以通过定义它的大小关系，再通过调用**sort**方法来进行排序。
&emsp;本题的讲解到此，谢谢！
