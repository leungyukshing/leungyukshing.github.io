---
title: QT打包版本问题
tags:
  - QT开发
abbrlink: 59101
date: 2018-08-04 19:05:47
---
## QT打包后版本问题
&emsp;&emsp;之前在用QT做数据库管理系统的时候，程序打包时遇到了程序无法运行的问题。具体情况为，使用`windeployqt`后在该文件夹运行程序，报错信息如下图：![报错信息](/images/报错信息.png)
<!-- more -->

&emsp;&emsp;遇到这个问题虽然很棘手，但解决并不难。上网搜了一下之后，我判断这个是版本问题，有可能是**64位程序引用了32位的文件**或者**32位程序引用了64位的文件**，也就是**版本不一致**的问题。
&emsp;&emsp;我们只需要在QT的`bin`文件夹中找到对应的版本文件（最有可能的是关于数据库的`.dll`文件），在`windeployqt`后的文件夹中替换对应的文件就可以了，如`libmysql.dll`这类文件，数据库版本与QT版本不同很容易导致这类错误。
&emsp;&emsp;如果对程序不是很熟悉，需要依靠一些工具来帮助我们找到版本不一致的文件，详细可参考 http://www.cnblogs.com/csuftzzk/p/windows_launch_error_0xc000007b.html。
&emsp;&emsp;关于解决QT打包中的版本问题就到这里了，希望能够帮到你，谢谢！


---
#### 参考资料：
1.http://www.cnblogs.com/csuftzzk/p/windows_launch_error_0xc000007b.html
2.https://www.cnblogs.com/lawliet12/p/6916410.html
