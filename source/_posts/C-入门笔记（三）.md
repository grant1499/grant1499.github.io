---
title: C++入门笔记（三）
tags:
  - C++入门
categories:
  - C++
mathjax: true
copyright: true
abbrlink: 63a29b8e
date: 2021-03-09 22:36:49
---

## 1.C++在非OOP方面的扩充

![image-20210309223740177](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232021671.png)

<!--more-->

## 2.函数重载回顾

注意：下图爆红代码，编译器无法正确匹配函数（存在多义性）。

![image-20210309230013279](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232021036.png)

正常运行。

![image-20210309230508180](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232021006.png)

## 3.作用域运算符`::`

![image-20210309230703134](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232021067.png)

## 4.new和delete

![image-20210309230729654](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232021375.png)

说明：这里p已经声明为int*指针，不需要特意强调new int，编译器会自动识别。

![image-20210313184252878](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232021038.png)

强调：

使用new可以动态开辟数组空间，但是必须提供数组每一维的大小，如：`int* p = new int[2][3][4]`；

new可以为简单变量初始化，如：`int* p = new int(5)`，但是不能对数组初始化；

使用delete释放动态数组空间，如：`delete []p`，不需要指出所删除数组的维数和大小。

## 5.引用

不允许建立void类型的引用，因为void本质上不是一个类型；

不能建立数组的引用，不能建立引用的引用。

demo1:

![image-20210313185008444](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232020915.png)

demo2:

![image-20210313185039548](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232020798.png)

## 6.对象数组与数组指针

### 对象数组及其初始化

demo1：

![image-20210316203312632](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232020718.png)

demo2：

![image-20210316203415021](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232020962.png)

demo3：

![image-20210316203445931](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232020581.png)

demo4：

![image-20210316203536971](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232020612.png)

### 对象指针

![image-20210316203610938](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232020992.png)

### this指针

作用基本同Java的this（这里其实没必要加上）

![image-20210316203645915](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232020805.png)

## 7.向函数传递对象

![image-20210316203831986](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232020268.png)

![image-20210316203924539](https://gitee.com/grant1499/blog-pic/raw/master/img/202110232020911.png)

## 8.构造函数与析构函数的执行次序

- 构造函数在对象被创建时调用，析构函数在对象被删除前调用。

- 后调用的对象先调用析构函数，与构造函数的执行顺序相反。

```c++
#include <iostream>
using namespace std;
class Point {
public:
	Point(int xx = 0, int yy = 0) { // 构造函数
		x = xx;
		y = yy;
		printf("%d\n",x);
	}
    ~Point();
private:
	int x, y;
};
Point::~Point(){
	printf("this:%d\n",this->x);
}

int main()
{
	Point p1(1,1), p2(2,2);
	return 0;
}
/*1
2
this:2
this:1*/
```

