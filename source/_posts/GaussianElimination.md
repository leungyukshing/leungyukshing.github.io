---
title: 高斯消元法之代码实现
abbrlink: 5180
date: 2018-03-15 23:45:59
tags:
---
## 前言
&emsp;在数值计算课程中学到了高斯消元法的详细算法过程，自己就用C++实现了一下。我将会把代码分块讲解，想直接看完整代码的可以直接到最后看。
<!-- more -->

## 高斯消元法讲解
### 变量定义
&emsp;高斯消元法主要是矩阵的运算，这里我们采用二维数组的形式存储矩阵，采用一维数组的形式存储向量。
```C++
int n; // 矩阵规模
double A[maxn][maxn]; // 用于存储A矩阵
double b[maxn]; // 用于存储b向量
double temp[maxn][maxn]; // 用于存储中间运算用的矩阵
double x[maxn]; // 用于存储答案的向量
```

### 数据输入与输出
读入矩阵
```C++
/*读取矩阵*/
void read() {
  cout << "Please input the scale of matrix n: ";
  cin >> n;
  cout << "|--------------------\n";
  cout << "|Please input the data: " << endl;

  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cin >> A[i][j];
    }
  }

  for (int i = 1; i <= n; i++) {
    cin >> b[i];
  }
}
```

每次消元后打印当前矩阵

```C++
/*显示中间步骤*/
void printTemp(int cases) {
  cout << "Times: " << cases << endl;
  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cout << setw(10) << A[i][j] << ' ';
    }
    cout << "| " << b[i] << endl;
  }
  cout << "END THIS SHOW-------------------------" << endl;
}
```

最终输出x向量

```C++
/*显示结果*/
void print() {
  cout << "|-------------------------------" << endl;
  cout << "Result: " << endl;
  for (int i = 1; i <= n; i++) {
    cout << "x[" << i << "] = " << x[i] << "   ";
  }
  cout << "\n|-------------------------------\n" << endl;
}
```
注意：为了便于实现算法，矩阵和向量的存储**下标均从1开始**。

### 顺序消元法
&emsp;顺序消元法分为两个步骤：第一是**消元**，把原始矩阵变成一个上三角矩阵；第二是**回代**，通过最后一个方程一直回代解出整个矩阵。
```C++
/*顺序消元法*/
void normalElimination() {
  // Elimination
  for (int k = 1; k < n; k++) {
    for (int i = k + 1; i <= n; i++) {
      temp[i][k] = A[i][k] / A[k][k];
      for (int j = 1; j <= n; j++) {
        A[i][j] -= temp[i][k] * A[k][j];
      }
    }
    for (int i = k + 1; i <= n; i++) {
      b[i] -= temp[i][k] * b[k];
    }

    printTemp(k);
  }

  // 回代
  x[n] = b[n] / A[n][n];
  for (int i = n - 1; i >= 1; i--) {
    x[i] = b[i];
    for (int j = i + 1; j <= n; j++) {
      x[i] -= A[i][j] * x[j];
    }
    x[i] /= A[i][i];
  }

  print();
}
```

### 列主元消元法
&emsp;首先我要解释一下为什么会有这个方法。在顺序消元法中，我们看到了这样一个语句`temp[i][k] = A[i][k] / A[k][k];`，这是用来求出第i行与主元行的比值。注意到，如果某一个主元`A[k][k]`特别小的话，被一个相对大的数除，就会存在精度损失的问题。为了克服这个问题，我们希望在一列中，每一次都尽可能地挑选**绝对值最大**的一个元素作为主元，这就是**列主元消元法**。
&emsp;该方法的**消元**和**回代**两个主要步骤和**顺序消元法**是一致的，不同的地方在于多了**选主元**和**换行**这两步。

```C++
/*列主元消去法*/
void maximalColumnPivotElimination() {
  for (int k = 1; k < n; k++) {
    // 选主元
    double ab_max = - 1;
    int max_ik;
    for (int i = k; i <= n; i++) {
      if (fabs(A[i][k]) > ab_max) {
        ab_max = fabs(A[i][k]);
        max_ik = i;
      }
    }

    // 交换行处理(判断0矩阵)
    if (ab_max < e) {
      cout << "det A = 0\n";
      break;
    }
    else if (max_ik != k) {
      double temp;
      // 交换矩阵的第k行和第max_ik行
      for (int j = 1; j <= n; j++) {
        temp = A[max_ik][j];
        A[max_ik][j] = A[k][j];
        A[k][j] = temp;
      }
      // 交换b中的第k行和第max_ik行
      temp = b[max_ik];
      b[max_ik] = b[k];
      b[k] = temp;
    }

    // 消元计算
    for (int i = k + 1; i <= n; i++) {
      temp[i][k] = A[i][k] / A[k][k];
      for (int j = 1; j <= n; j++) {
        A[i][j] -= A[k][j] * temp[i][k];
      }
      b[i] -= temp[i][k] * b[k];
    }
    printTemp(k);
    if (k < n-1) {
      continue;
    }
    else {
      if (abs(A[n][n]) < e) {
        cout << "det A = 0\n";
        break;
      }
      else {
        x[n] = b[n] / A[n][n];
        for (int i = n - 1; i >= 1; i--) {
          x[i] = b[i];
          for (int j = i + 1; j <= n; j++) {
            x[i] -= A[i][j] * x[j];
          }
          x[i] /= A[i][i];
        }
        print();
      }
    }
  }
}
```

## 完整代码
```C++
#include <iostream>
#include <iomanip>
#include <cmath>
using namespace std;
#define e 0.00000001
#define maxn 50

int n;
double A[maxn][maxn];
double b[maxn];
double temp[maxn][maxn];
double x[maxn];

/*读取矩阵*/
void read() {
  cout << "Please input the scale of matrix n: ";
  cin >> n;
  cout << "|--------------------\n";
  cout << "|Please input the data: " << endl;

  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cin >> A[i][j];
    }
  }

  for (int i = 1; i <= n; i++) {
    cin >> b[i];
  }
}

/*显示中间步骤*/
void printTemp(int cases) {
  cout << "Times: " << cases << endl;
  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cout << setw(10) << A[i][j] << ' ';
    }
    cout << "| " << b[i] << endl;
  }
  cout << "END THIS SHOW-------------------------" << endl;
}

/*显示结果*/
void print() {
  cout << "|-------------------------------" << endl;
  cout << "Result: " << endl;
  for (int i = 1; i <= n; i++) {
    cout << "x[" << i << "] = " << x[i] << "   ";
  }
  cout << "\n|-------------------------------\n" << endl;
}

/*顺序消元法*/
void normalElimination() {
  // Elimination
  for (int k = 1; k < n; k++) {
    for (int i = k + 1; i <= n; i++) {
      temp[i][k] = A[i][k] / A[k][k];
      for (int j = 1; j <= n; j++) {
        A[i][j] -= temp[i][k] * A[k][j];
      }
    }
    for (int i = k + 1; i <= n; i++) {
      b[i] -= temp[i][k] * b[k];
    }

    printTemp(k);
  }

  // 回代
  x[n] = b[n] / A[n][n];
  for (int i = n - 1; i >= 1; i--) {
    x[i] = b[i];
    for (int j = i + 1; j <= n; j++) {
      x[i] -= A[i][j] * x[j];
    }
    x[i] /= A[i][i];
  }

  print();
}

/*列主元消去法*/
void maximalColumnPivotElimination() {
  for (int k = 1; k < n; k++) {
    // 选主元
    double ab_max = - 1;
    int max_ik;
    for (int i = k; i <= n; i++) {
      if (fabs(A[i][k]) > ab_max) {
        ab_max = fabs(A[i][k]);
        max_ik = i;
      }
    }

    // 交换行处理(判断0矩阵)
    if (ab_max < e) {
      cout << "det A = 0\n";
      break;
    }
    else if (max_ik != k) {
      double temp;
      // 交换矩阵的第k行和第max_ik行
      for (int j = 1; j <= n; j++) {
        temp = A[max_ik][j];
        A[max_ik][j] = A[k][j];
        A[k][j] = temp;
      }
      // 交换b中的第k行和第max_ik行
      temp = b[max_ik];
      b[max_ik] = b[k];
      b[k] = temp;
    }

    // 消元计算
    for (int i = k + 1; i <= n; i++) {
      temp[i][k] = A[i][k] / A[k][k];
      for (int j = 1; j <= n; j++) {
        A[i][j] -= A[k][j] * temp[i][k];
      }
      b[i] -= temp[i][k] * b[k];
    }
    printTemp(k);
    if (k < n-1) {
      continue;
    }
    else {
      if (abs(A[n][n]) < e) {
        cout << "det A = 0\n";
        break;
      }
      else {
        x[n] = b[n] / A[n][n];
        for (int i = n - 1; i >= 1; i--) {
          x[i] = b[i];
          for (int j = i + 1; j <= n; j++) {
            x[i] -= A[i][j] * x[j];
          }
          x[i] /= A[i][i];
        }
        print();
      }
    }
  }
}

int main() {
  while (1) {
    read();
    //normalElimination();
    maximalColumnPivotElimination();
  }
}
```

高斯消元法的代码实现讲解到这里结束了，谢谢！
