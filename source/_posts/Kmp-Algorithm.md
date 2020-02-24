---
title: KMP算法
date: 2020-02-24 15:09:03
---

## Intro

&emsp;&emsp;KMP算法是由D.E.**K**nuth、J.H.**M**orris和V.R.**P**ratt三位共同提出的，是一种高效的字符串匹配算法。该算法优化了主串指针的回溯步骤，大大提高了算法的效率。在这篇博客，我将讲述KMP的原理和实现。

<!-- more -->

## Algorithm

### What I go for?

&emsp;&emsp;我们需要解决的是什么问题呢？简单来说就是字符串匹配问题。给定一个**主串**和一个**模式**，算法需要返回这个模式在主串中出现的位置，通常是返回成功匹配的第一个下标，如果没有匹配则返回-1.

### Why not Brute Force?

&emsp;&emsp;看完题目后，大多数人都能够想到的一个简单做法就是暴力搜索。从头到尾遍历主串，以每一个位置作为头去匹配模式串。但是这里面有一个效率的问题，比方说主串`ABCDAB ABCDABD`和模式串`ABCDABD`，在暴力求解的过程中，`ABCD`这个短串后被匹配两次，也就是说在主串中，出现第一个`ABCD`的时候我们会匹配一次，出现`ABCDABD`的时候也会匹配一次。需要注意，第二次前三个字符的匹配是可以优化的，因为经过第一次的匹配后，我们已经知道`ABCDAB`这六个字符，也就说我们没有必要往下一个字母`B`去匹配（因为一定匹配不上），而是可以从下一个出现`AB`的地方开始匹配，这样就跳过了已经比较过的位置，减少匹配的次数了。这就是KMP的思路。

### KMP

&emsp;&emsp;从上面的分析能够看出，KMP的关键在于知道需要跳过多少个字符。因为模式串我们是知道的，所以不需要在匹配过程中才摸索到这个规律，而是可以在匹配前就知道这个规律。这种加速信息实际上就是在模式串里面重复串的信息，所以我们可以有一个预处理。需要用一个数组去描述这个模式串内的重复情况。

&emsp;&emsp;这个数组一般命名为`next`，定义为：位置$i$上的值为以这个位置上的字符作为**前缀**和**后缀**的**最长共有元素**的长度。详情请参考[KMP详解]([http://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html](http://www.ruanyifeng.com/blog/2013/05/Knuth–Morris–Pratt_algorithm.html))。于是根据这个重复的信息，加上我们已经匹配好的长度，就可以计算出需要跳过的位数：已经匹配的字符数-最后一个匹配字符对应的next值。

### Code

```c++
#include <iostream>
#include <string>
using namespace std;

int* getNext(string str) {
  int size = str.size();
  int* next = new int[size];
  for (int i = 0; i < size; i++) {
    next[i] = -1;
  }
  next[1] = 0;
  int j = 0;
  int k = -1; // pointer
  while (j < size - 1) {
    if (k == -1 || str[j] == str[k]) {
      j++;
      k++;
      next[j] = k;
    }
    else {
      k = next[k];
    }
  }

  // for (int i = 0; i < size; i++) {
  //   cout << next[i] << " ";
  // }
  return next;
}

int kmp(string str, string pattern) {
  int i = 0;
  int j = 0;
  int* next = getNext(pattern);
  while (i < str.size() && j < pattern.size()) {
    if (str[i] == pattern[j]) {
      i++;
      j++;
    }
    else {
      if (j == 0) {
        i++;
      }
      else {
        j = next[j];
      }
    }
  }
  // all matched
  if (j == pattern.size()) {
    return i - j;
  }
  // not match
  return -1;
}

int main() {
  string str1, pattern;
  cout << "string: ";
  cin >> str1;
  cout << "pattern: ";
  cin >> pattern;
  int idx = kmp(str1, pattern);
  cout << "first match index: " << idx << endl;
  return 0;
}
```

![kmp result](/images/kmp.png)

---

## 小结

&emsp;&emsp;KMP的关键就在于如何获取next这个数组，里面包含的就是模式串内部的重复信息。有关KMP的分享到这里，感谢您的支持，谢谢！