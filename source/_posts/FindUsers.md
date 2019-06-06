---
title: 找到喜好值为k的用户个数
abbrlink: 10614
date: 2019-06-05 00:19:43
tags:
---

## 题目

&emsp;&emsp;字节跳动的经典笔试题：为了不断优化推荐效果，今日头条每天要存储和处理海量数据。假设有这样一种场景：我们对用户按照它们的注册时间先后来标号，对于一类文章，每个用户都有不同的喜好值，我们会想知道某一段时间内注册的用户（标号相连的一批用户）中，有多少用户对这类文章喜好值为k。因为一些特殊的原因，不会出现一个查询的用户区间完全覆盖另一个查询的用户区间。

<!-- more -->

&emsp;&emsp;**输入描述**：第1行为`n`，代表用户的个数。第2行为`n`个整数，第`i`个代表用户标号为`i`的用户对某类文章的喜好度。第3行为一个正整数`q`代表查询的组数。第4行到第(3+q)行，每行包括三个整数`l`，`r`，`k`代表一组查询。查询的是在`[l,r]`中的用户对这类文章喜好值为`k`的用户的个数。

&emsp;&emsp;**输出描述**：一共`q`行，每行一个整数代表喜好值为`k`的用户的个数。

```
Input:
5
1 2 3 3 5
3
1 2 1
2 4 5
3 5 3

Output:
1
0
2
```

---

## 思路

&emsp;&emsp;最基本的思路是对于给出的查询区间的每个用户进行遍历，判断其喜好度是否与`k`相等，然后再进行计数。这种做法效率不高，它的效率受到了查询区间大小的限制。

&emsp;&emsp;第二种方法更加高效，做法如下。我们首先同时记录用户的标号和喜好度（使用自定义结构体）。然后对喜好度进行排序。这样喜好度一样的用户就会放在相邻的位置。然后我们建立一个map，它的key是user的喜好度，value是在数组中相同喜好度用户的开始下标。然后进行查询操作，我们首先通过喜好度`k`在map中找到开始的下标，然后逐一遍历喜好度为`k`的用户，判断其标号是否在查询范围内。

&emsp;&emsp;两种做法实质上是操作的顺序不一样。第一种做法是先判断范围再判断喜好度，第二种做法是先判断喜好度再判断范围。先后顺序的不同是提升效率的关键，因为喜好度相同的用户数量相对较少，这样遍历可以大大减少循环的次数。

---

## 实现

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <map>
using namespace std;

struct Node {
  int index;
  int key;
  Node(int index_, int key_) {
    index = index_;
    key = key_;
  }
};

bool cmp(const Node &a, const Node &b) {
  return a.key < b.key;
}

int main() {
  int n;
  cin >> n;
  vector<Node> user;
  for (int i = 1; i <= n; i++) {
    int tmp;
    cin >> tmp;
    Node node(i, tmp);
    user.push_back(node);
  }
  // 从小到大
  sort(user.begin(), user.end(), cmp);
  //cout << user[0].key << endl;
  //cout << user[1].key << endl;

  map<int, int> m; // <key, first_idx>
  for (int i = 0; i < user.size(); i++) {
    int key = user[i].key;
    if (m.count(key) > 0) {
      continue;
    }
    else {
      m[key] = i;
    }
  }
  int q;
  cin >> q;
  while (q--) {
    int l, r, k;
    cin >> l >> r >> k;
    int count = 0;
    if (l > r || m.count(k) < 1) {
      cout << 0 << endl;
      continue;
    }
    int first_idx = m[k];
    for (int i = first_idx; i < user.size() && user[i].key == k; i++) {
      if (user[i].index >= l && user[i].index <= r) {
        count++;
      }
    }
    cout << count << endl;
  }
  
  return 0;
}
```

## 总结

&emsp;&emsp;这道题目看起来比较困难，因为用第一种做法很容易超时。但是理清思路后，先判断喜好度，再判断范围，这样的话就能够减少循环的次数。希望这篇博客能够帮助到你，谢谢！