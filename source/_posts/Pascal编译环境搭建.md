---
title: Pascal编译环境搭建
abbrlink: 48289
date: 2019-01-19 15:59:34
tags:
---

## Pascal编译环境搭建

这篇博客就带大家搭建一个Pascal编译环境（Win10下）

<!-- more -->

## 下载

首先上网下载一个free pascal编译器。下载地址为：https://www.freepascal.org/download.html。

下载与自己电脑想匹配的版本。如果是64位的机器，32和64位的版本都要下载。

## 安装并配置环境变量

然后将下载好的两个程序都安装，都选择默认设置就可以了，安装在同一个文件夹下。比如我安装在了`C:\FPC\`。打开`C:\FPC\3.0.4\bin\i386-win32`你就能找到一个`fpc.exe`文件，这个就是pascal的编译程序。

打开配置环境变量，将这个文件夹路径添加进去，这样就能够配置成功了。

## 运行第一个Pascal程序

这里给出一个简单的pascal程序用于测试环境。

文件名`hello.pas`：

```pascal
begin
  writeln('Hello world!');
end.
```

然后用命令行打开这个文件所在的目录，输入如下命令：

```bash
$ fpc hello.pas
```

运行结果：![编译pascal](/images/pascal_compile.png)

然后你就能看到文件夹下多了两个文件`hello.o`和`hello.exe`。前者是中间的二进制文件，后者是可执行文件。直接运行exe：

```bash
$ hello.exe
```

可以看到在命令行中输出”Hello world!"字样。

![运行pascal](/images/pascal_run.png)

---

至此，我们就完成了Pascal编译环境的搭建了，并运行了我们第一个Pascal程序。希望这篇博客能够帮助到你，谢谢您的支持！