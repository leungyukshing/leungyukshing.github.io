---
title: 消除字符串中相邻的0和1
abbrlink: 38055
date: 2019-05-29 11:36:21
tags:
---

## 题目

&emsp;&emsp;一道经典的字符串题目，但是最近看到有一种新的思路，所以拿出来分享。

&emsp;&emsp;题目描述：给定一个仅包含0或1的字符串，现在可以对其进行一种操作，当有两个相邻的字符其中一个是0另外一个是1的时候，可以消除这两个字符。这样的操作一直进行下去，直到找不到相邻的0和1为止，问这个字符串经历了操作以后的最短长度。

<!-- more -->

## 思路

&emsp;&emsp;首先给出第一种解法，直接按照题目的要求一步一步走，遍历字符串，查看当前位置`i`的字符和下一个位置`i+1`的字符能否消除，每次清除后都从头来过（一定要重头来过才能应付如0011）的情况。最后统计剩下的字符串的长度就是答案了。这种做法思路简单，符合题目的描述，但是复杂度过高。

&emsp;&emsp;第二种做法比较巧妙，有点像是从结果倒推的思维。第一，因为消除是成对消除，所以消去0的数量一定等于消去1的数量；第二，最后剩下的字符串如果不为空，只能有一种数字，要么是0，要么是1。基于这两个想法，我们可以分别统计字符串中0和1的数量`one_number`和`zero_number`，他们中较小的一个就是能够抵消的数量，最后再用原长度`size`减去较小的数量的**两倍**，得出最终答案。即`size - 2* min(one_number, zero_number)`。

## 实现

第一种方法：

```c++
// Method1
int findMin1(string& s) {
  int i = 0;
  while (i < (int)(s.size() - 1)) {
    if ((s[i] == '0' && s[i + 1] == '1') || (s[i] == '1' && s[i + 1] == '0')) {
      s.erase(i, 2);
      i = 0; // reset
    }
    else {
      i++;
    }
  }
  return s.size();
}
```

第二种方法：

```c++
// Mothod2
int findMin2(string& s) {
  int zero_number = 0, one_number = 1;
  int size = s.size();
  for (int i = 0; i < size; i++) {
    if (s[i] == '0') {
      zero_number++;
    }
    else {
      one_number++;
    }
  }
  return (size - 2 * min(zero_number, one_number));
}
```

主程序：

```c++
int main() {
  string str;
  cin >> str;
  cout << findMin2(str) << endl;
  return 0;
}
```

---

## 小结

&emsp;&emsp;这道题也算是字符串操作中比较经典的题目了。第二种做法明显比第一种做法效率高很多，但是比较难想到。感觉这类题目还是要多积累才能找到技巧。希望这篇博客能够帮助到你，谢谢！

