---
title: 翻转字符串
abbrlink: 50361
date: 2019-06-07 13:57:40
tags:
---

## 题目描述

&emsp;&emsp;这是一道经典的字符串题目，要求翻转字符串的每一个部分，每一个部分由`.`分隔开，但部分内部保持原来的顺序，并且只能够在原字符串上操作，不能使用额外的内存空间（允许用一个char的空间），也不能够使用`<string>`的`reverse()`函数。

<!-- more -->

---

## 思路

&emsp;&emsp;字符串翻转的题目相信大家都见过不少，最基本的思路就是首尾交换，不断向中间遍历。分部分翻转其实也不难，我们在写python或者java的时候经常会用到`split()`，将一个字符串按照分隔符划分成一个字符串数组，但是在这里题目只允许在原串上操作。我们只好换个思路了。

&emsp;&emsp;既然翻转一次不行，那我们干脆就翻转两次。第一次对整个字符串进行翻转，这样使得部分之间的顺序可以翻转。然后我们再对每个部分的内部进行翻转，还原成原来的顺序。这样一分析，是不是就很简单了。

---

## 实现

&emsp;&emsp;整个程序有很多字符串翻转的操作，我们干脆写一个`reverse()`函数，方便调用。

```c++
#include <iostream>
#include <string>
using namespace std;
/* 自定义反转函数 */
void reverse(string& str, int begin, int end) {
  // 将str的begin到end部分翻转
  for (int i = 0; i <= (end - begin) / 2; i++) {
    char tmp = str[begin + i];
    str[begin + i] = str[end - i];
    str[end - i] = tmp;
  }
}

/* 按区域反转字符串 */
void reverseString(string& str) {
  int size = str.size();
  reverse(str, 0, size - 1);
  int j;
  for (int i = 0; i < size; i=j+1) {
    for (j = i; j < size; j++) {
      if (str[j] == '.' || j == size - 1) {
        break;
      }
    }
    if (j == size - 1) {
      j++;
    }
    reverse(str, i, j - 1);
  }
}

int main() {
  string str;
  cin >> str;
  reverseString(str);
  cout << str << endl;
  return 0;
}
```

运行结果：![ReverseString](/images/reversestring.png)

---

# 小结

&emsp;&emsp;这道题是我在面试过程中碰到的，之前没有复习到，短时间内完成coding还是有点困难，尤其是一些边界问题，稍微不注意就容易出错。希望通过分享给大家一些帮助，谢谢！