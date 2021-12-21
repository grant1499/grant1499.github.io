---
title: C++入门笔记（十）
tags:
  - C++入门
  - 模板
categories:
  - C++
mathjax: true
copyright: true
abbrlink: 48b5732
date: 2021-04-15 21:07:03
---

## 1.什么是模板

![image-20210416202641797](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232022078.png)

<!--more-->

为什么要使用模板？

因为使用重载还是有重复代码，使用模板可以写一个通用的函数适用多种不同的数据类型。

![image-20210416213008855](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232022899.png)

## 2.函数模板与模板函数

**函数模板**就是建立一个通用函数，其函数返回类型和形参类型不具体指定，用一个虚拟类型来代表。

在调用函数时系统会根据实参的类型（模板实参）来取代模板中的虚拟类型从而实现不同的函数功能。

![image-20210416213921364](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232022873.png)

使用函数模板时必须将类型参数实例化，类型参数前要加上关键字typename或者class，推荐加上typename，class容易与类搞混。

**typename后面的类型参数必须实例化。**

![image-20210416215058349](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232022921.png)

![image-20210416215118498](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232022760.png)

**实际上函数模板提供了一类函数的抽象，它经过实例化而生成的具体函数成为模板函数。**

![image-20210416215300861](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232022755.png)

定义两种类型参数的函数模板：

![image-20210416215736766](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232022839.png)

注意点：

![image-20210416215654841](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232022284.png)

重载的函数模板例子：

![image-20210416215918724](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232022034.png)

函数模板与同名的非模板函数也能重载。

这时的调用顺序是：先寻找一个参数完全匹配的非模板函数，找到了就调用它，否则寻找一个匹配的函数模板，实例化产生模板函数，调用它。

![image-20210416220231627](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232021188.png)

## 3.类模板与模板类

**类模板**就是建立一个通用类，其数据成员、成员函数的返回类型和形参类型不具体指定，用一个虚拟类型来代表。使用类模板定义对象时，系统会根据实参的类型来取代模板中的虚拟类型从而实现了不同类的功能。

![image-20210416220749449](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232021312.png)

还是建议使用typename关键字。

![image-20210416222050662](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232021251.png)

![image-20210416222039232](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232021900.png)

![image-20210416222122639](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232021371.png)

用数组实现的栈的例子：

![image-20210416222507375](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232021373.png)

含有两个类型参数的模板类的例子：

![image-20210416222605659](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232021912.png)