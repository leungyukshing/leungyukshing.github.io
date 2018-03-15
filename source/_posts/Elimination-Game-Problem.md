---
title: 消除类游戏
tags:
  - CCF
abbrlink: 14145
date: 2018-03-15 00:46:49
---
## 消除类游戏问题
### 问题描述
&emsp;消除类游戏是深受大众欢迎的一种游戏，游戏在一个包含有n行m列的游戏棋盘上进行，棋盘的每一行每一列的方格上放着一个有颜色的棋子，当一行或一列上有连续三个或更多的相同颜色的棋子时，这些棋子都被消除。当有多处可以被消除时，这些地方的棋子将同时被消除。
&emsp;现在给你一个n行m列的棋盘，棋盘中的每一个方格上有一个棋子，请给出经过一次消除后的棋盘。
&emsp;请注意：一个棋子可能在某一行和某一列同时被消除。
<!-- more -->

### 输入格式
&emsp;输入的第一行包含两个整数n, m，用空格分隔，分别表示棋盘的行数和列数。
&emsp;接下来n行，每行m个整数，用空格分隔，分别表示每一个方格中的棋子的颜色。颜色使用1至9编号。

### 输出格式
&emsp;输出n行，每行m个整数，相邻的整数之间使用一个空格分隔，表示经过一次消除后的棋盘。如果一个方格中的棋子被消除，则对应的方格输出0，否则输出棋子的颜色编号。

### 样例输入1
> 4 5
> 2 2 3 1 2
> 3 4 5 1 4
> 2 3 2 1 3
> 2 2 2 4 4

### 样例输出1
> 2 2 3 0 2
> 3 4 5 0 4
> 2 3 2 0 3
> 0 0 0 4 4

### 样例说明1
&emsp;棋盘中第4列的1和第4行的2可以被消除，其他的方格中的棋子均保留。

### 样例输入2
> 4 5
> 2 2 3 1 2
> 3 1 1 1 1
> 2 3 2 1 3
> 2 2 3 3 3

### 样例输出2
> 2 2 3 0 2
> 3 0 0 0 0
> 2 3 2 0 3
> 2 2 0 0 0

### 样例说明2
&emsp;棋盘中所有的1以及最后一行的3可以被同时消除，其他的方格中的棋子均保留。

### 评测用例规模与约定
&emsp;所有的评测用例满足：1 ≤ n, m ≤ 30。

## 解决方法
### 分析
&emsp;这个问题本身不难，只是一个简单的对矩阵行和列进行遍历，寻找**3个或3个以上**连续相同的数字。其中一个难点是，由于一个棋子可以同时被行和列消去，因此如果只在一个棋盘中操作的话，就会很不方便。所以我提供的一种解决方法就是另外新建一个布尔类型的矩阵用于记录每一个方格是否应该被消去。
&emsp;然后就是分别对行列进行扫描，判断是否有连续的颜色，这里要注意的是，要对边界情况进行判断。

### 代码实现
```C++
#include <iostream>
using namespace std;

int main() {
  int n,m;
  cin >> n >> m;
  int map[n][m];
  bool elimination[n][m];
  for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
      cin >> map[i][j];
    }
  }

  for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
      elimination[i][j] = false;
    }
  }

  int color;
  int count;

  /*行扫描*/
  for (int i = 0; i < n; i++) {
    color = map[i][0];
    count = 1;
    for (int j = 1; j < m; j++) {
      if (map[i][j] == color) {
        count++;
      }
      else {
        if (count >= 3) {
          while (count) {
            elimination[i][j - count] = true;
            count--;
          }
        }
        count = 1;
        color = map[i][j];
      }
      /*边界情况*/
      if (j == m-1) {
        if (count >= 3) {
          while (count) {
            elimination[i][j - count + 1] = true;
            count--;
          }
        }
      }
    }
  }

  /*列扫描*/
  for (int i = 0; i < m; i++) {
    color = map[0][i];
    count = 1;
    for (int j = 1; j < n; j++) {
      if (map[j][i] == color) {
        count++;
      }
      else {
        if (count >= 3) {
          while (count) {
            elimination[j - count][i] = true;
            count--;
          }
        }
        count = 1;
        color = map[j][i];
      }
      /*边界情况*/
      if (j == n-1) {
        if (count >= 3) {
          while (count) {
            elimination[j - count + 1][i] = true;
            count--;
          }
        }
      }
    }
  }

  /*输出*/
  for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
      if (elimination[i][j]) {
        cout << "0 ";
      }
      else {
        cout << map[i][j] << " ";
      }
    }
    cout << endl;
  }

  return 0;
}
```
本题的讲解到这里结束了，谢谢！
