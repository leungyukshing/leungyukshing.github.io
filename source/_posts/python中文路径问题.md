---
title: python中文路径问题
tags:
  - python
abbrlink: 23075
date: 2018-06-25 20:37:37
---
## 关于pip install flask命令中文路径的问题
&emsp;&emsp;最近在做一个后端服务器，需要使用flask，使用pip安装的时候，命令行中输入`pip install flask`，然后出现了如下的报错信息：
```
UnicodeDecodeError: 'ascii' codec can't decode byte 0xc0 in position 0: ordinal not in range(128)
```
&emsp;&emsp;这个是关于**中文的编码问题**，因为win10下比较多用户使用自己的中文名作为盘符，因此路径中涉及到了中文（windows下使用的是GBK编码格式）。不过大家不用担心，只需要修改几个地方就可以了。
<!-- more -->

## 解决方法
&emsp;&emsp;找到python所在的文件位置，我的是`C:\Python27\Lib\site-packages\pip`，找到`basecommand.py`并打开，在下面的位置添加相应的代码，重启cmd即可。如果还有其他文件有类似的情况，只需要在对应的`.py`文件中加入相同的代码就可以解决。
![](/images/py.png)
注意，新版本的python在setdefaultencoding后需要reload。
