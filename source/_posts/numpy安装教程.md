---
title: Numpy安装教程
tags:
  - python
abbrlink: 3369
date: 2018-08-22 00:09:55
---

## NumPy安装
&emsp;&emsp;NumPy函数库是python开发环境中的一个独立模块，它可以用于存储和处理大型矩阵，在人工智能的方面经常会使用到。如果python运行后有报错信息`No module named numpy`，这就说明当前的开发环境缺少NumPy模块，我们需要安装。
&emsp;&emsp;步骤如下:
  + 1.下载NumPy数据库，根据自己python的版本以及操作系统选择对应的安装包，下载地址：[点击进入下载地址](https://www.lfd.uci.edu/~gohlke/pythonlibs/)
  + 2.将下载好的安装包copy至**python的安装目录下C:\Pyhon27\Scripts**
  + 3.进入命令行，输入`pip2.7 install C:\Pyhon27\Scripts\numpy-1.14.5+mkl-cp27-cp27m-win32.whl`
  + 4.若安装成功会提示**successfully installed**.
  + 5.进入Python Shell，输入`from numpy import *`，如果没有报错信息，则安装成功！
