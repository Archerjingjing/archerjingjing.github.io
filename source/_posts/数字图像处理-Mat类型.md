---
title: 数字图像处理-Mat类型
date: 2020-02-18 23:42:07
tags: 数字图像处理
categories: 学习路程
---
# Mat类的一些注意事项
1· Mat：：eye（）：表示开辟对角线的单位矩阵

2· Vec对象：可以使用【】符号来进行数组操作
```
访问Mat内矩阵的三个方法：
1· at<type>（）函数。
2· iterator（迭代器）：MatIterator_<type>对象作为后面iterator.begin<type>()和iterator.end<type>()函数的范围常量。
3· 通过指针方法：用.ptr<type>()函数获取像素的首个指针。
```
3· 可以用Range类来选取范围：Range(x,y)或Range::all()函数

4· 对于感兴趣区域ROI：可以直接Mat(Range(x,y))

5· 取对角线Mat::diag(int):0则为主对角线

6· Mat由于对运算符进行了重载，所以可以直接进行算数表达式

7· 读取Mat对象时要检测是否打开 Mat.empty()

8· Mat_类
 > Mat_类是对Mat的包装，使其不用重视后面对Mat类的数据类型的维护
 ,如果使用 Mat_类，那么就可以在变量声明时确定元素的类型， 访问元素时不再需要指定元素类型
```
%在变量声明时指定矩阵元素类型
Mat_<uchar> M1 = (Mat_<uchar>&)M;
%不需指定元素类型，语句简洁38
uchar * p = M1.ptr(i);
%直接使用 Matlab 风格的矩阵元素读写，简洁
M1(i,j) = d1;
```

9· Mat的内存管理
> 在创建Mat类型时一般都不会重复申请内存，而是共享引用。由两个数据部分组成：矩阵头和指向数据的指针（会多申请4个字节用于记录矩阵的引用次数）

10· randu()函数：填充随机数

11· 数据的读取
> 一般建议为png：有压缩，且是无损     bmp：无损没有压缩   jpeg：有压缩但是有损。
 VedioCapture对象和VedioWriter


