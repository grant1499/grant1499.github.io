---
title: R语言学习笔记（一）
tags:
  - R入门
categories:
  - R
mathjax: true
copyright: true
abbrlink: fd673177
date: 2021-05-24 15:43:46
---

## 1.R语言资源推荐

推荐入门书籍：[R语言编程](https://www.grantdrew.top/posts/900c9142.html)

推荐入门学习网站：https://www.w3cschool.cn/r/r_overview.html，其实菜鸟教程也有

推荐入门视频教程：https://www.bilibili.com/video/BV19x411X7C6?p=7&share_source=copy_web

<!--more-->

## 2.基本操作命令

获取或设置当前路径

```R
getwd()
setwd("D:/R-4.0.3/tests")
# 注意：路径中的\必须用/或\\代替
```

赋值

```R
# <-，R特有
x <- 3
# =，推荐使用
x = 3
```

R是一种基于对象的的向量化编程语言，既是面向对象也是面向函数。

基本运算

![image-20210524214853067](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232141663.png)

逻辑运算；

![image-20210524214917013](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232141147.png)

## 3.R语言数据结构

![image-20210601203233160](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232141671.png)

![image-20210601203256315](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232141258.png)

## 4.向量

向量是R语言的基本数据类型，常用的向量有：数值向量、逻辑向量和字符向量，以及复数向量。

原子向量有6种类型：逻辑型（logical），整型（integer），双精度型（double），字符型（character），

复数型（complex）和原始型（raw）。

单个向量中的数据必须拥有相同的类型。

### 4.1数值向量

由数值组成的向量，单个数值就是长度为1的数值向量。如：`x = 1.2`

常用函数：

- numeric(length)函数用于创建全为0的指定长度length的数值向量。
- c()函数用于将多个对象合并返回数值向量。如：`x = c(12,123.23,"asd",TRUE)`将返回一个字符向量。
- seq()函数：或“:“，length.out可以简写为length

![image-20210531193747724](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232141061.png)

- rep()函数：

![image-20210531194714539](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232141561.png)

例子：

```R
> 2:3 # 同seq(5)或者seq(1,5)
[1] 2 3
> 1:5
[1] 1 2 3 4 5
> 2:3 + 1:5
[1] 3 5 5 7 7
Warning message:
In 2:3 + 1:5 : 长的对象长度不是短的对象长度的整倍数
# R中两个不同长度的向量运算时，短的会自动循环补齐以配合长的
> seq(1,11,3)
[1]  1  4  7 10
> seq(2,length.out = 4)
[1] 2 3 4 5
--------------
>  1:10
 [1]  1  2  3  4  5  6  7  8  9 10
> seq(1,10,by=0.5)# 步长为0.5
 [1]  1.0  1.5  2.0  2.5  3.0  3.5  4.0  4.5  5.0  5.5  6.0
[12]  6.5  7.0  7.5  8.0  8.5  9.0  9.5 10.0
> seq(1,10,length=7)# 长度为7
[1]  1.0  2.5  4.0  5.5  7.0  8.5 10.0
> seq(along.with=c(1,2,5,4))
[1] 1 2 3 4
> seq(along.with=c(1,2,5,4),length=3)# 长度还是由along.with决定
[1] 1 2 3 4
> seq(2,3,along.with=c(1,2))
[1] 2 3
> seq(2,3,along.with=c(1,2,1,1))
[1] 2.000000 2.333333 2.666667 3.000000
---------------
> rep(2:5,rep(2,4))# rep(2,4)的值是2,2,2,2向量
# 它作为time参数，规定2:5中对应重复次数为2,2,2,2
[1] 2 2 3 3 4 4 5 5
> rep(2:5,c(2,1,1,3))# c(2,1,1,3)作为time参数，规定2:5中对应重复次数为2,1,1,3
[1] 2 2 3 4 5 5 5
> rep(1:4,each=2,len=4)
[1] 1 1 2 2
> rep(1:4,each=2,len=10)
 [1] 1 1 2 2 3 3 4 4 1 1
> x = c(12,3,2,4)
> length(x)
[1] 4
---------
x = c(1,2,3)
assian("x",c(1,2,3))# 和上面等价
```

### 4.2逻辑向量

是一组逻辑值(TRUE/FALSE或者简写为T/F)的向量。

![image-20210531200145685](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232141307.png)

![image-20210601211549041](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232141313.png)

![image-20210601211627366](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232141223.png)

```R
> x <- c(1,3,4,0,-2)
> x <= 0
[1] FALSE FALSE FALSE  TRUE  TRUE
> class.stu = c("一","二","三")
> week = c("一","二","五")
> class.stu %in% week
[1]  TRUE  TRUE FALSE
> which(x <= 0)# 返回下标，注意下标从1开始
[1] 4 5
> which(! class.stu %in% week)
[1] 3
```

### 4.3字符向量

是一组字符串组成的向量。（单双引号括起来）

paste/paste0 函数， 用于连接字符（向量）， paste 可以设置连接字符，默认以空格作为连接字符； paste0 以空字符串连接字符，不能设置 sep 值。collapse 参数可以实现用 sep 连接后的字符向量的元素间的再次连接。

```R
> paste("a", "b") # 默认以空格连接字符 a 和 b， [1] "a b"
paste("a", "b", "c")
[1] "a b c"
> paste("a", "b", sep = "=") # 用“=” 连接字符 a 和 b[1] "a=b"
#连接多个元素
> paste("a", 1:5, sep = "") # 1:5 中的每个元素与 a 相连[1] "a1" "a2" "a3" "a4" "a5"
> paste("a",1:5,".pdf", sep = "") # 批量生成文件名[1] "a1.pdf" "a2.pdf" "a3.pdf" "a4.pdf" "a5.pdf"
# 有总结说：sep 连接的是元素间的扩展，collapse 是一种元素间的折叠坍缩
> a <- c("一", "两", "三四")
> paste(a, "片", sep="")
[1] "一片"   "两片"   "三四片"
> paste(a, "片", sep="", collapse="")
[1] "一片两片三四片"
```

### 4.4向量的访问

![image-20210601212011478](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232141284.png)

![image-20210601214815362](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232141706.png)

```R
> x
[1]  2  3  4  0  5 -2  3  0
> which(x==3)
[1] 2 7
> x = c(1,2,3,4)
> x[c(2,1)]
[1] 2 1
> x[x^2 - x>=3]
[1] 3 4
> which.max(x)# 返回最大值下标
[1] 4
> which.min(x)# 返回最小值下标
[1] 1
```

![image-20210601221546509](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232142186.png)

```R
> x = c(a=1,b=4,c=6)# 创建向量时对元素命名
> x["c"]# 提取对象的子集，有点像字典
c 
6 
> which(x==4)
b 
2 
> x[["a"]]# 提取对象中的元素
[1] 1
```

### 4.5向量排序

![image-20210601222040044](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232142027.png)

![image-20210601222150146](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232142491.png)

```R
> x = c(1,5,8,2,9,7,4)
> sort(x)
[1] 1 2 4 5 7 8 9
> order(x) # 默认升序，返回元素排好序对应位置的索引
[1] 1 4 7 2 6 3 5
> x[order(x)] # == sort(x)
[1] 1 2 4 5 7 8 9
> rank(x) # 默认升序
[1] 1 4 6 2 7 5 3
> rev(x) # 反转
[1] 4 7 9 2 8 5 1
```

