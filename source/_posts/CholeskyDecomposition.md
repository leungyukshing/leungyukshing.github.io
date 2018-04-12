---
title: Cholesky分解
mathjax: true
abbrlink: 50154
date: 2018-04-11 17:52:53
tags:
---
## 矩阵的Cholesky分解（平方根法）
基于之前讲过的矩阵的**LU分解**，**Cholesky分解**是对于**对称正定矩阵**的一种优化算法。对于在计算机上解决此类矩阵，该方法有很大的优势。
<!--more-->
## 算法分析
设**A**为对称矩阵，且**A**的所有顺序主子式均不为0，根据**LU分解**可以知道，**A**可以唯一地分解成为**LU**形式，在这里，我们利用**A**的对称性，对**U**进行再分解：
$$
    U=
    \begin{bmatrix}
    u_{11} \\
    & u_{22} \\
    & & \ddots \\
    & & & u_{nn} \\
    \end{bmatrix}
    \begin{bmatrix}
    1 & \frac{u_{12}}{u_{11}} & \cdots & \frac{u_{1n}}{u_{11}} \\
     & 1 & \cdots & \frac{u_{2n}}{u_{22}} \\
     & & \ddots & \vdots \\
     & & & 1 \\
    \end{bmatrix} = DU_0,
$$
其中**D**为对角矩阵，**$U_0$**为单位上三角矩阵，于是
$$
A = LU = LDU_0
$$
又因为
$$
A = A^T = U_0^T(DL^T)
$$
由分解的唯一性可以得到：
$$
U_0^T = L
$$
故对于对称矩阵**A**，存在唯一分解：
$$
A = LDL^T，其中L为单位下三角矩阵，D为对角矩阵
$$
且根据**A**正定矩阵，可以知道任意非零向量**x**都有$x^TAx> 0$,取x = （1, 0, ..., 0)，则第一个结果为$d_1 > 0$，以此类推，$d_i$均大于0。
于是有
$$
D=
    \begin{bmatrix}
    d_{1} \\
    & & \ddots \\
    & & & d_{n} \\
    \end{bmatrix} =
    \begin{bmatrix}
    \sqrt{d_1} \quad \\
    & & \ddots \\
    & & & \sqrt{d_n} \quad \\
    \end{bmatrix}
    \begin{bmatrix}
    \sqrt{d_1} \quad \\
    & & \ddots \\
    & & & \sqrt{d_n} \quad \\
    \end{bmatrix} = D^{\frac{1}{2}}D^{\frac{1}{2}},
$$
所以
$$
A = LDL^T = LD^{\frac{1}{2}}D^{\frac{1}{2}}L^T = (LD^{\frac{1}{2}})(LD^{\frac{1}{2}}) = L_1L_1^T，其中L_1 = (LD^{\frac{1}{2}})为下三角矩阵。
$$

我们已经证明了是存在这样一个$A = L_1L_1^T$的分解的，接下来我们就推导出L的元素。
$$
A=
    \begin{bmatrix}
    l_{11} \\
    l_{21} & l_{22} \\
    \vdots & \vdots & \ddots \\
    l_{n1} & l_{n2} & \cdots & l_{nn} \\
    \end{bmatrix}
    \begin{bmatrix}
    l_{11} & l_{21}& \cdots & l_{n1} \\
     & l_{22} & \cdots & l_{n2} \\
     & & \ddots & \vdots \\
     & & & l_{nn} \\
    \end{bmatrix},
$$
由矩阵乘法可以推出
$$
a_{ij} = \sum_{k=1}^{n}l_{ik}l{jk} = \sum_{k=1}^{j-1}l_{ik}l_{jk} + l_{jj}l_{ij},
$$
于是可以得到解对称正定方程组**Ax = b**的平方根法计算公式：
对于$j=1,2, \cdots, n$
$$
(1) l_{jj} = (a_{jj} - \sum_{k=1}^{j-1}l_{jk}^2)^{\frac{1}{2}};
$$
$$
(2)l_{ij} = (a_{ij} - \sum_{k=1}^{j-1}l_{ik}l_{jk}) / l_{jj}, i = j+1, \cdots, n.
$$
求解**Ax = b**，即求解两个三角形方程组：
①$Ly= b$，求**y**;②$L^Tx = y$，求**x**。
$$
(3) y_i = (b_i - \sum_{k=1}^{i-1}l_{ik}y_k) / l_{ii}, i = 1,2,\cdots,n.
$$
$$
(4) x_i = (b_i - \sum_{k=i+1}^nl_{ki}x_k) / l_{ii}, i = n,n-1,\cdots,1.
$$

## 代码分析
### 变量定义
```C++
int n;
double A[maxn][maxn] = {0};
double L[maxn][maxn] = {0};
double U[maxn][maxn] = {0};
double b[maxn] = {0};
double y[maxn] = {0};
double x[maxn] = {0};
```
使用二维数组存储矩阵，使用一维数组存储向量。
注意，由于A为对称矩阵，因此在存储时可以只存储**A**的下三角部分。但是在本次代码中没有这样做。

### 输入与输出
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


/*显示结果*/
void print() {
  cout << "L:" << endl;
  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cout << setw(4) << L[i][j] << " ";
    }
    cout << endl;
  }

  cout << "y:" << endl;
  for (int i = 1; i <= n; i++) {
    cout << y[i] << " ";
  }
  cout << endl;

  cout << "|-------------------------------" << endl;
  cout << "Result: " << endl;
  for (int i = 1; i <= n; i++) {
    cout << "x[" << i << "] = " << x[i] << "   ";
  }
  cout << "\n|-------------------------------\n" << endl;
}
```
分别有处理输入、输出分解后结果和输出最终解的三个方法。

### Cholesky分解
```C++
void Cholesky() {
  /*得到L矩阵*/
  for (int j = 1; j <= n; j++) {
    double sum1 = 0;
    for (int k = 1; k <= j - 1; k++) {
      sum1 += L[j][k] * L[j][k];
    }
    L[j][j] = sqrt(A[j][j] - sum1);

    sum1 = 0;

    for (int i = j + 1; i <= n; i++) {
      for (int k = 1; k <= j - 1; k++) {
        sum1 += L[i][k] * L[j][k];
      }
      L[i][j] = (A[i][j] - sum1) / L[j][j];
    }
  }

  /*求解两个方程组*/
  for (int i = 1; i <= n; i++) {
    double sum2 = 0;
    for (int k = 1; k <= i - 1; k++) {
      sum2 += L[i][k] * y[k];
    }
    y[i] = (b[i] - sum2) / L[i][i];
  }

  for (int i = n; i >= 1; i--) {
    double sum3 = 0;
    for (int k = i + 1; k <= n; k++) {
      sum3 += L[k][i] * x[k];
    }
    x[i] = (y[i] - sum3) / L[i][i];
  }

  print();
}
```
根据上面的算法分析，直接可以很容易地写出代码
### 完整代码
```C++
#include <iostream>
#include <iomanip>
#include <cmath>
using namespace std;
#define maxn 50

int n;
double A[maxn][maxn];
double L[maxn][maxn];
double b[maxn];
double y[maxn];
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


/*显示结果*/
void print() {
  cout << "L:" << endl;
  for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n; j++) {
      cout << setw(4) << L[i][j] << " ";
    }
    cout << endl;
  }

  cout << "y:" << endl;
  for (int i = 1; i <= n; i++) {
    cout << y[i] << " ";
  }
  cout << endl;

  cout << "|-------------------------------" << endl;
  cout << "Result: " << endl;
  for (int i = 1; i <= n; i++) {
    cout << "x[" << i << "] = " << x[i] << "   ";
  }
  cout << "\n|-------------------------------\n" << endl;
}

void Cholesky() {
  /*得到L矩阵*/
  for (int j = 1; j <= n; j++) {
    double sum1 = 0;
    for (int k = 1; k <= j - 1; k++) {
      sum1 += L[j][k] * L[j][k];
    }
    L[j][j] = sqrt(A[j][j] - sum1);

    sum1 = 0;

    for (int i = j + 1; i <= n; i++) {
      for (int k = 1; k <= j - 1; k++) {
        sum1 += L[i][k] * L[j][k];
      }
      L[i][j] = (A[i][j] - sum1) / L[j][j];
    }
  }

  /*求解两个方程组*/
  for (int i = 1; i <= n; i++) {
    double sum2 = 0;
    for (int k = 1; k <= i - 1; k++) {
      sum2 += L[i][k] * y[k];
    }
    y[i] = (b[i] - sum2) / L[i][i];
  }

  for (int i = n; i >= 1; i--) {
    double sum3 = 0;
    for (int k = i + 1; k <= n; k++) {
      sum3 += L[k][i] * x[k];
    }
    x[i] = (y[i] - sum3) / L[i][i];
  }

  print();
}

int main() {
    read();
    Cholesky();
}
```
LU分解法求解$Ax = b$的**算法复杂度**大约是**O($\frac{1}{6} n^3$)**，约为直接**LU分解**计算量的一半，是一个可观的优化。

至此，矩阵的Cholesky分解算法及代码的讲解已经结束了，谢谢！

---
#### 参考资料：
1.数值分析（第5版）  李庆扬，王能超，易大义 *编*
