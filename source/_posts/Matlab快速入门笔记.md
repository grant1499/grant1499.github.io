---
title: Matlab快速入门笔记
tags:
  - Matlab入门
categories:
  - - Matlab
mathjax: true
copyright: true
abbrlink: 284e5fc1
date: 2021-03-07 19:48:41
---

## 1.初始Matlab

![image-20210307201346455](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232133850.png)

点击新建脚本即创建`.m`文件。

<!--more-->

**命令行窗口**

![按向上箭头弹出历史命令](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232133654.png)

`clear`：清空工作区

`clc`：清空命令行

## 2.Matlab帮助系统

![image-20210307203526202](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232134314.png)

### 查询函数：

![image-20210307203605322](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232134010.png)

### 在线查询：

![image-20210307205132363](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232134018.png)

### 模糊搜索：

![image-20210307205443076](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232134409.png)

### 内存变量列表（who）：

### 内存变量信息（whos）：

![image-20210307210116394](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232134710.png)

Matlab的最小数据单元就是矩阵，所以size为1*1.

### 变量检测函数（exist）：

![image-20210307210444329](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232134527.png)

### 文件操作：

![image-20210307211255022](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232134857.png)

## 3.基础知识

MATLAB的代码执行如果返回结果但是没有写变量来保存 默认保存在 ans 变量中 。

注意：自己定义变量最好不要命名为ans。

### 3.1命令

![image-20210307212506649](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232134217.png)

![image-20210307213023170](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232134676.png)

![image-20210307213045734](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232134622.png)

![image-20210307213914912](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232135696.png)

### 3.2数据类型

![image-20210307214128238](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232134324.png)

![image-20210307214034914](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232135773.png)

常量：

![image-20210307214359948](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232135789.png)

注意：Matlab可以改变常量的值

![image-20210307214721096](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232135144.png)

数值类型包括整型、 浮点型 、 符号型 、 复数类型等 。

![image-20210307215356971](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232135958.png)

![image-20210307215409394](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232135965.png)

复数：与实数类似，虚数单位用 i 表示即可 。

