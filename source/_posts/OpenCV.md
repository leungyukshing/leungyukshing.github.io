---
title: OpenCV使用总结——C++版本
abbrlink: 47522
date: 2019-08-03 00:17:59
tags:
---

## Intro

&emsp;&emsp;[OpenCV](<https://opencv.org/>)是一个基于BSD许可（开源）发行的跨平台计算机视觉库，能够运行在Linux、Windows、Android和MacOS这些当下的主流操作系统中。很多时候我们在处理图像的过程中都会用到OpenCV这个库，在这篇博客我就来总结一下我使用OpenCV的一些体验。

<!-- more -->

&emsp;&emsp;使用OpenCV前有必要了解一下opencv这个库的组成。它是由一系列的C函数和少量的C++类构成，同时提供了python、ruby、matlab等多种语言的接口。它的最大优势是执行速度快，对于支持实时应用非常友好。

---

## 总结

### 1.获取图片信息

假设我们有一个Mat类型的数据，我们想获取图片的大小等属性：

```c++
Mat src = MatMat(rows, cols, CV_8UC3);
int channels = src.channels(); // Get channels
int rows = temp.rows; // Get height
int cols = temp.cols; // Get Width
```

### 2.对图片进行resize

在处理图片的时候，很多情况下我们需要对图片进行压缩和resize操作，使得图片大小符合我们的算法输入，opencv提供了一个很方便的resize方法。

```c++
Mat src = Mat(ORIGINAL_HEIGHT,ORIGINAL_WIDTH, CV_8UC4);

// Create new Mat
Size dsize = Size(NEW_HEIGHT, NEW_WIDTH);
Mat des = Mat(NEW_HEIGHT, NEW_WIDTH, CV_8UC4)

resize(src, des, dsize); // resize Mat
```

### 3.RGB通道的坑

opencv中Mat类型的RGB通道排序并不是顺序排序的，也就是说我们获取到每一个像素点的像素值是一个数组，这个数组的内容是该像素点在各个通道上的值，**但并不是按照RGB排列的，而是按照BGR排列的**。所以我们处理的时候需要手动调整顺序。这里我将图片按照WxHxC的方法来提取成一个一维数组。

```c++
Mat temp = Mat(rows, cols, CV_8UC4);

// change Mat to 1-dim array
uchar* array = new uchar[length];

// extract value pixel-by-pixel
for (int i = 0; i < rows; i++) {
    for (int j = 0; j < cols; j++) {
        array[(i * cols + j ) * 3] = temp.at<Vec4b>(i, j)[2]; // Red Channel
        array[(i * cols + j ) * + 1] = temp.at<Vec4b>(i, j)[1]; // Green Channel
        array[(i * cols + j ) * + 2] = temp.at<Vec4b>(i, j)[0]; // Blue Channel
    }
}
```

### 4.添加矩形框

我们可以在一个Mat上添加一个矩形框，方法有很多，我推荐的做法是使用两个点来确定矩形的位置：**左上角和右下角**。注意，在opencv的图片矩阵中，图片的左上方才是坐标原点。

```c++
Mat picture = Mat(rows, cols, CV_8UC4);

// draw box
rectangle(picture, Point(xmin, ymin), Point(xmax, ymax), Scalar(0, 0, 255), 2, 8); //利用左上角坐标及宽高
```

### 5.添加文字

做识别框的时候，我们需要在框的附近标注一下基本的信息，这时候就需要用到文字。文字的产生分为两步：首先生成文字，然后使用**左下角坐标**来定位。这里假设在上面生成的矩形框的左上角位置添加文字标识。

```c++
int fontface = FONT_HERSHEY_COMPLEX_SMALL;
double fontscale = 1;
int thickness = 3;
int baseline = 0;

// Make Text
Size textsize = getTextSize(text, fontface, fontscale, thickness, &baseline);

// Make Text box
rectangle(*picture, Point(xmin, ymin - textsize.height - 10), Point(xmin + textsize.width + 6 , ymin), Scalar(0, 0, 255), 2, 8);
putText(*picture, text, Point(xmin + 3, ymin - 4), FONT_HERSHEY_COMPLEX_SMALL, 1, Scalar(255, 255, 255), 0);
```

TBC。。。

---

## Reference

1. [openCV Mat Class Reference](<https://docs.opencv.org/3.1.0/d3/d63/classcv_1_1Mat.html>)
2. [resize 用法详解](<https://blog.csdn.net/mangobar/article/details/78327341>)
3. [rectangle 用法详解](<https://blog.csdn.net/fuping07/article/details/52586495>)
4. [putText() 用法](<https://blog.csdn.net/qq_38392229/article/details/92709871>)
5. [getTextSize() 参数解释](<https://blog.csdn.net/u010970514/article/details/84075776>)